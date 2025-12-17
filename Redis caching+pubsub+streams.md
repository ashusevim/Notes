# Caching

- Caching is a way to store frequently used data so it can be used later for the same data request

## Redis basics

- Redis is an in-memory data structure store, that keeps the data in RAM rather than on a disk, which also means read and write operations get faster, perfect for caching.

### 1. Key value model

- Redis works as key-value store. we can think of it like a large JSON object or a Python dictionary.

### 2. TTL (Time to Live)

- it lets you set an expiration time on a key. when the time runs out redis automatically deletes the key.

**Question.** 

If you are caching a **product feed** (e.g., a list of today's "Hot Deals"), what is the risk of *not* setting a TTL, and how would that affect the user experience?

ans. 

**stale data. Showing "Hot Deals" that are no longer valid is a way to frustrate users.**

There is also a hidden infrastructure risk: Memory Bloat. So without TTL, our cache would eventually fill up with old, and useless keys. Since RAM is limited, Redis would eventually run out of space and crash

## Caching patterns

- we needs to make sure the cache and DB stay in sync.

### Two of the most common patterns are

1. **write-through:** application writes data to the cache and database at the same time. operation isn’t considered “success” until both acknowledge it.
2. **write-back:** application writes data to the cache only. and the cache then updates thee DB later in the background

                                          ****

## pub/sub + streams

- Redis isn’t just for storing keys
- it is also good for messaging
- this is crucial for real-time features

### 1. Pub/sub (publish/subscribe)

we can think of this like a **Radio station**

- **publisher:** sends a message to a “channel”
- **subscriber:** listens to that channel
- Fire and forget. If no one is listening when the message is sent, the message is lost forever. It is not stored.

### 2. Redis Streams

It is like a whatsApp group chat log

- **producer:** adds an entry to the stream
- **consumer:** reads from the stream
- Persistence. Messages are stored in a log. if a consumer goes offline and come back later, they can “catch up” and read the messages they missed.

## Cache invalidation strategies

> *"There are only two hard things in Computer Science: cache invalidation and naming things."* — Phil Karlton
> 

The hardest part is knowing when to take out data so users don’t see wrong data.

### Time-based invalidation (TTL)

- we set a timer, nothing else
- easy to implement & self-cleaning

disadvantage: there is a window of “staleness” if you update the database at minute 1, the cache will still show old data until minute 5

### Event-based invalidation (Explicit delete)

- so whenever our application makes some changes in the database, it immediately sends a command to the redis to delete that specific key.
- Data is almost consistent.
- The next time user asks for that data it won’t be in the cache, forcing a fresh fetch from the database (cache-aside).
- and we have to ensure every place in our code that updates the DB also deletes the cache key.

### versioning (key mutation)

- instead of changing the value, you change the key.
- **Old key:** `product_feed_v1`
- **new key:** `product_feed_v2`
- great for large datasets

## Edge caching (CDNs)

**Latency:** if your redis server is in **New York**, but your user is in **Tokyo. so t**he data still has to travel halfway across the world via fiber optic cables. That takes time (~200ms)

**Solution: Edge caching (CDN)** services like **Cloudflare** or **Fastly** have servers (points of presence, or Pops) in hundred of cities worldwide

1. Origin server: your man server/redis in new york
2. edge server: a CDN server sitting in Tokyo, London, Mumbai

**How it works:**

- **First Request:** The Tokyo user asks for the Product Feed. The Tokyo Edge server doesn't have it, so it asks your New York server (Origin), saves a copy, and serves it to the user.
- **Second Request:** The *next* user in Tokyo asks for the feed. The Tokyo Edge server serves it instantly (5ms). It never touches your New York server.

## User/session vs Global Cache

| **Feature** | **Type** | **Example Data** | **Risk** |
| --- | --- | --- | --- |
| **Global Cache** | Shared | Product Feeds, Pricing, News Articles | Low. If it leaks, everyone was supposed to see it anyway. |
| **Session Cache** | Private | Shopping Cart, User Profile, "My Orders" | **CRITICAL.** If you cache this globally, User A might log in and see User B's credit card info! which is horrific RIGHT |
- Global Keys usually look like: `product:123`
- session keys usually look like: `user:555:cart` ( always include the user ID in the key)

## Mini-project: cache product feed + rate limit

### **Goal:** Build a system that serves a product feed, caches it for speed, but also rate limits users so they don’t crash our API during the sale.

### **Architecture**

1. **Check Rate Limit:** Does User X have "tokens" left to make a request? (Redis Key: `rate_limit:user:X`)
2. **Check Cache:** Do we have the `product_feed` in Redis?
3. **If Miss:** Fetch from DB → Save to Redis (Write-Through/Cache-Aside) → Return to User.
4. **If Hit:** Return Redis data immediately.

### Rate Limiter Key (limit users to 10 requests per minute)

we have two specific commands in our toolkit.

1. **The counter:** `INCR`
- It is used to increment a number stored at a key.
- **syntax:** `INCR key_name`
- `If the key doesn’t exist yet, redis is smart enough to create it, set it to 0 and increment it to 1`

```
INCR counter
returns (integer) 1
```

1. **The timer:** `EXPIRE`
- It tells the redis to delete the key automatically after a specific number of seconds.
- **Syntax:** `EXPIRE key_name seconds`

```
// expire my key(counter) after 60 seconds
EXPIRE counter 60
(integer) 1 # returns 1 if successful
```

- let’s say `User X` gets excited and hits the refresh button 10 more times very quickly. so on the 11th request, the `INCR` command will return `11`.
- if we run `EXPIRE` again on the 11th request, we reset the timer back to **60 seconds**.
    
    Imagine a user hitting the API once every 30 seconds.
    
    - **0s:** Request 1. (TTL set to 60s).
    - **30s:** Request 2. If we run `EXPIRE` again, TTL goes *back* to 60s.
    - **60s:** Request 3. TTL resets to 60s again...
    
    The key would **never expire**, and the user’s count would never reset to 0!
    

```tsx
import express, { Request, Response, NextFunction } from "express";
import { createClient } from "redis";

const app = express();
const client = createClient();

await client.connect();

const rateLimiter = async (req: Request, res: Response, next: NextFunction) => {
    // we would be using user ip address as the unique identifier
    const userIP = req.ip;
    const key = `rate_limit:${userIP}`;

    // 1. Increment the key (INCR)
    // 2. If it's the first request, set the expiry to 60s (EXPIRE)
    // 3. If the count > 10, return a 429 error
    // 4. Otherwise, allow the request (next())
    const requestCount = await client.incr(key);

    if (requestCount == 1) {
        await client.expire(key, 60);
    } else if (requestCount > 10) {
        res.status(429).json({
            message: "too many requests",
        });
    } else {
        next();
    }
};

app.use(rateLimiter);

app.get("/products", async (req: Request, res: Response) => {
    const key = "products_feed";

    try {
        const cachedData = await client.get(key);

        if (cachedData) {
            console.log("Cache HIT");
            res.status(200).json(JSON.parse(cachedData));
            // stop the execution as we got the data from the cache
            return;
        }

        console.log("Cache MISS");
        const db_data = await getProductFeedFromDB();

        await client.setEx(key, 60, JSON.stringify(db_data));

        res.status(200).json(db_data);
    } catch (error) {
        console.log("Server error");
        res.status(500).json({
            message: "Internal server error",
        });
    }
});

app.listen(3000, () => {
    console.log(`server running on port: 3000`);
});
```

### Implementing Real-time Updates

for now, the user has to refresh the page if want to see the price changed. It is called “PULL” technology.

Pub/sub allows us to use “PUSH” technology.

**The Scenario:** Imagine an Admin updates the price of the "iPhone 15" in the database. We want to **instantly** notify the console of the running server (or a connected user) that "Price dropped!" without them sending a request.

**We need two roles:**

1. **publisher:** The one sending the message (e.g., an Admin script).
2. **Subscriber:** The one listening (e.g., your running server).

> A client cannot be a subscriber and do regular work (like GET/SET) at the same time efficiently. We need a **duplicate client** just for listening.
> 

```tsx
app.post("/orders", async (req: Request, res: Response) => {
    const { productId, quantity } = req.body;

    if (!productId || !quantity) {
        console.log("ProductId or quantity are missing!");
        res.status(400).json({
            message: "productid or quantity are missing!",
        });
    }

    try {
        const orderDetails = await client.xAdd("order_stream", "*", {
            productId: String(productId),
            quantity: String(quantity),
        });

        res.status(201).json({
            message: "Order Received Successfully",
            orderId: orderDetails,
        });
    } catch (error) {
        console.log(error);
        res.status(500).json({
            message: "could not process order",
        });
    }
});
```

- Streams are best for things that cannot be lost.

Building a consumer(we might have pile of orders sitting in the stream, we need worker to process them)

worker: Runs  in the background, pulls events and processes them.

Now, what if we get 10,000 orders per second, that one worker will get fu**ked.

and if we run the processOrder() (implementation of worker) twice, both will read the same message. 

Solution is to use consumer groups, which allows to spin up 5 workers.

order 1 → worker 1

order 2 → worker 2

and so on…

it automatically handles load balancing.

### The Concept: The Kitchen Analogy

- **Pub/Sub (What we did first):** Like a generic announcement system. "Order #1 is ready!" Everyone hears it. If you have 5 waiters listening, all 5 try to grab the same plate. full Chaos.
- **Single Stream Worker:** One chef trying to cook *every* order. If 1000 orders come in, that poor chef collapses.
- **Consumer Groups (The New Thing):** You hire a team of 5 chefs (the "Group").
    - Redis acts as the **Head Chef**.
    - It hands Order 1 to Chef A.
    - It hands Order 2 to Chef B.
    - It hands Order 3 to Chef C.
    - If Chef A crashes, Redis remembers "Chef A never finished Order 1" and gives it to someone else later.

```jsx
const processOrders = async () => {
    const workerClient = client.duplicate();
    await workerClient.connect();

    const key = "order_stream";
    const group = "our_app_group";
    const consumerName = "worker-1"; // UUID or pod name in production

    console.log("Group worker loading");

    try {
        await workerClient.xGroupCreate(key, group, "$", { MKSTREAM: true });
        console.log("created consumer group!");
    } catch (error) {}

    while (true) {
        try {
            const response = (await workerClient.xReadGroup(
                group,
                consumerName,
                [
                    {
                        key: key,
                        id: ">", // > meaning giving new undelivered messages
                    },
                ],
                {
                    BLOCK: 0, // wait forever
                    COUNT: 1,
                },
            )) as any;

            if (response) {
                const myStream = response[0];

                for (const msg of myStream.messages) {
                    console.log(`Processing order: ${msg.id}`);
                    console.log(`ProductL: ${msg.message.productId}`);

                    // simulating DB processing time for tasks like Save to DB, email user
                    await new Promise((r) => setTimeout(r, 1000));

                    await workerClient.xAck(key, group, msg.id);
                    console.log(`Acknowledged: ${msg.id}`);
                }
            }
        } catch (error) {
            console.log("Worker error: ", error);
        }
    }
};
```

Redis acts as a **Line manager.** It ensures no double work(one order is never sent to multiple workers) and load balancing(splits the work evenly so no single worker gets overwhelmed).

Horizontal scaling: in case your app gets too slow, you don’t need a faster computer; you just add more servers. Redis automatically uses it.
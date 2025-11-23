# Exponential Back-off

- It is an algorithm used to space out between the repeated attempts of failed operation, for example. network request
- so retrying at a constant rate, it delays the next attempt exponentially

## How it works?

1. **Initial failure**: so the operation fails
2. **first retry**: waits for the base interval(for example 1 second) and try to connect again
3. **Second retry**: if it fails, the previous interval time increases exponentially and retry
4. **Subsequent retries**: it if continue to fail, keep doubling the waiting time exponentially(1 second, 2 seconds, 4 seconds, 8 seconds, 16 seconds, 32 seconds and so on). until the operation succeeds or maximum limit reaches

## Key Benefits

- **Server Protection:** It gives a struggling server or service time to recover without being overwhelmed.
- **Increased Stability:** It makes the entire system more resilient and less prone to
- **Reduced Network Traffic:** It avoids flooding a network with useless, repetitive requests.
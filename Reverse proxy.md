# Reverse proxy

- server sits between client(probably web browser) and back-end server.
- routing client requests to the appropriate back-end server and then forwarding the response back to the client.
- Improve security, performance, and scalability by hiding the back-end 
servers, balancing loads across multiple servers, caching content, and 
filtering traffic.

## How the Reverse Proxy works?

1. when the client sends a request to the server, it goes first to the proxy server instead of back-end server directly
2. proxy server forwards request to one of the back-end servers, often distributing the load to improve performance and reliability
3. The back-end server responds to the proxy server, which may inspect or modify the response before sending it back to the user

## Benefits of using Reverse Proxies

1. Load Balancing
2. Enhanced Security
3. Improved performance
4. SSL termination
5. Centralized Authentication
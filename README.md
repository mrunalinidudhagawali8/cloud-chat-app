 [Client]
    |
    v
[API Gateway] ---> [Auth Service] --- DB (User + Token)
    |
    |--- REST -> [Chat Service] --- PostgreSQL/MongoDB
    |
    |--- WS -> [WebSocket Gateway] <-> Redis
                                |
                                v
                          [Message Broker] ---> [Delivery Worker] --> WS Push / DB Save / Retry


| Service               | Responsibilities                                                   |
| --------------------- | ------------------------------------------------------------------ |
| **API Gateway**       | Routes requests, handles throttling, TLS termination               |
| **Auth Service**      | Login, register, JWT, refresh tokens                               |
| **Chat Service**      | Business logic for sending/receiving, group handling               |
| **WebSocket Gateway** | Maintains persistent connection, maps users to sockets             |
| **Message Broker**    | Buffers messages, guarantees at-least-once delivery                |
| **Delivery Worker**   | Processes queue, delivers messages, retries, handles backoff & DLQ |
| **PostgreSQL/Mongo**  | Stores chat history, user data                                     |
| **Redis** (opt)       | Stores user-socket mapping, message caching                        |

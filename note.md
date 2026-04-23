# SuperShop Microservices Platform

A cloud-native e-commerce application built with .NET, demonstrating modern architectural patterns and robust inter-service communication.

## 🏗️ Architecture Choices
*   **Microservices Architecture**: The domain is decomposed into independent, loosely-coupled services (`Product`, `Cart`, `Discount`, `Ordering`).
*   **Database-per-Microservice**: Every service owns its data natively (PostgreSQL isolated per logic).
*   **CQRS & MediatR**: Segregates read queries and write commands for optimal scalability (`Product`, `Cart`, `Ordering`).
*   **Vertical Slice Architecture / Minimal APIs**: Groups code around features rather than technical layers for `Product`, `Cart`.
*   **Clean Architecture (DDD)**: `Ordering` leverages Domain-Driven Design separating core entities from infrastructure dependencies.
*   **YARP API Gateway**: Centralized reverse-proxy to handle inbound routing, load balancing, and rate limiting securely.

## 🚀 Inter-Service Communication Methods

### Asynchronous Messaging (RabbitMQ via MassTransit)
*   **Pattern**: Publisher-Subscriber (Pub/Sub)
*   **Use Case**: Finalizing a purchase in a non-blocking, resilient way.
*   **Flow**: When a user checks out, the `Cart` service publishes a `CartCheckoutEvent` to RabbitMQ. The `Ordering` service asynchronously consumes this event to finalize the order creation without forcing the user to wait or risking data loss if the order service temporarily drops.

### Synchronous RPC Calling (gRPC)
*   **Pattern**: Request-Response 
*   **Use Case**: Fetching required data internally at lightning-fast speeds.
*   **Flow**: High-performance internal request. When processing a cart, the `Cart` service uses gRPC over HTTP/2 to query the `Discount.Grpc` service for active coupons, ensuring minimal latency when updating shopping totals.

## 🛠️ Tech Stack
*   **.NET 10.0** (C# 14)
*   **PostgreSQL** (Entity Framework Core & Marten)
*   **Redis** (Distributed Caching for Cart)
*   **RabbitMQ & MassTransit** (Message Bus)
*   **gRPC** (High Performance RPC)
*   **YARP** (Yet Another Reverse Proxy - Gateway)
*   **Docker Compose** (Container Orchestration)
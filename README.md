# 🛒 E-commerce Backend — Microservices Architecture

Hey! I'm **Prem**, a Backend Developer specializing in Java, Spring Boot, and integration platforms.
This project is my hands-on implementation of a production-inspired **e-commerce backend** using a microservices architecture.

I built this to go beyond tutorial-level code — real service boundaries, async messaging, distributed tracing, and containerized deployment. Everything's wired up and ready to run with a single Docker Compose command.

---

## 🧩 Architecture Overview

Each service owns a specific business domain and communicates either synchronously (via OpenFeign/API Gateway) or asynchronously (via Kafka). No shared databases — each service manages its own MySQL instance.

```
Client
  └──▶ API Gateway (:9191)
          ├──▶ Identity Service   (Auth, JWT, Roles)
          ├──▶ Product Service    (Catalog, Inventory, Redis Cache)
          ├──▶ Order Service      (Orders, Tracking, History)
          ├──▶ Payment Service    (Transactions, Invoices)
          └──▶ Email Service      (Kafka Consumer → Notifications)

Service Registry (Eureka :8761) — All services register here
Zipkin (:9411)                  — Distributed tracing across services
```

---

## ⚙️ Tech Stack

| Layer | Technology |
|---|---|
| Core Framework | Spring Boot 3.x |
| Microservices Utilities | Spring Cloud (Gateway, Eureka) |
| Service-to-Service | OpenFeign |
| Async Messaging | Apache Kafka |
| Auth & Security | Spring Security + JWT |
| Database | MySQL (per-service) |
| Caching | Redis |
| Tracing | Zipkin |
| Containerization | Docker & Docker Compose |

---

## 🚀 Getting Started

### Prerequisites

- Docker & Docker Compose
- Git

### Run Everything

```bash
git clone https://github.com/Premkumar36032/shopverse-microservices.git
cd shopverse-microservices
docker-compose up -d
```

Verify all containers are up:

```bash
docker-compose ps
```

### Service Endpoints

| Service | URL |
|---|---|
| API Gateway | http://localhost:9191 |
| Eureka Dashboard | http://localhost:8761 |
| Zipkin Tracing | http://localhost:9411 |

---

## 🔐 API Usage

### Register a New User

```bash
POST http://localhost:9191/api/v1/auth/register

{
  "name": "Prem",
  "email": "prem@example.com",
  "password": "yourpassword"
}
```

### Login & Get Token

```bash
POST http://localhost:9191/api/v1/auth/token

{
  "email": "prem@example.com",
  "password": "yourpassword"
}
```

Use the returned JWT in the `Authorization: Bearer <token>` header for all protected routes.

### Sample Order Flow

```bash
# 1. Browse products
GET http://localhost:9191/api/v1/products

# 2. Place an order
POST http://localhost:9191/api/v1/orders
Authorization: Bearer <token>

# 3. Check order status
GET http://localhost:9191/api/v1/orders/{orderId}
Authorization: Bearer <token>
```

---

## 🗄️ Database Access

Each service runs its own MySQL instance on a different port:

| Service | Port |
|---|---|
| Identity Service DB | 3307 |
| Product Service DB | 3308 |
| Order Service DB | 3309 |
| Payment Service DB | 3310 |

Connect to any instance:

```bash
mysql -h 127.0.0.1 -P 3307 -u root -p
```

---

## 📬 Kafka Event Flow

Order placement triggers an async event chain:

```
Order Service  ──publishes──▶  order.created (Kafka Topic)
                                      │
                               Email Service (Consumer)
                                      │
                               Sends confirmation email
```

This decouples the order flow from notification logic — if the email service is down, orders still go through.

---

## 🔍 Distributed Tracing

Every request gets a trace ID that flows across services. Open Zipkin at [http://localhost:9411](http://localhost:9411) to trace latency, spot bottlenecks, and debug cross-service failures.

---

## 📁 Project Structure

```
ecommerce-microservices/
├── api-gateway/
├── service-registry/
├── identity-service/
├── product-service/
├── order-service/
├── payment-service/
├── email-service/
├── docker-compose.yml
└── README.md
```


---

## ⚠️ Notes

- This project is built for learning and portfolio purposes
- Some environment-specific configs (ports, credentials) may need adjustment
- Make sure Docker is running before executing `docker-compose up`


---

*Built by [Prem](https://github.com/your-username) — Backend Developer | Java · Spring Boot · MuleSoft*

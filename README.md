# E-Commerce Order Management System

A full-stack microservices-based e-commerce platform built with **Java**, **Spring Boot**, **Kafka**, **Angular**, and **MySQL**.

## Project Overview

This is a complete e-commerce application featuring customer-facing storefront, admin dashboard, event-driven microservices architecture with Kafka, JWT-based authentication, and Docker deployment.

## Business Problem

Build an online shopping platform where customers can register, browse products, manage their cart, checkout with various payment methods, and track orders — while admins manage products, inventory, orders, and payments through a dedicated dashboard.

## Key Features

### Customer Features
- ✅ User registration and JWT authentication
- ✅ Product browsing with search, filter, sort, and pagination
- ✅ Shopping cart management (add, update, remove)
- ✅ Multi-step checkout with address and payment selection
- ✅ Simulated payment processing (Card, UPI, Net Banking, COD)
- ✅ Order tracking with visual timeline
- ✅ Order cancellation with inventory release
- ✅ Notification center with read/unread management
- ✅ Profile management

### Admin Features
- ✅ Dashboard with real-time statistics
- ✅ Product management (CRUD)
- ✅ Category management
- ✅ Inventory monitoring with stock status alerts
- ✅ Order management with valid state transitions
- ✅ Payment tracking and filtering

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Backend | Java 17, Spring Boot 3.2.5 |
| Frontend | Angular 17, TypeScript |
| Database | MySQL 8.0 |
| Message Broker | Apache Kafka |
| API Gateway | Spring Cloud Gateway |
| Authentication | Spring Security + JWT |
| ORM | Spring Data JPA / Hibernate |
| Build | Maven |
| Container | Docker + Docker Compose |
| API Docs | SpringDoc OpenAPI (Swagger) |

## Architecture

```
┌─────────────────────────────────────────────────┐
│                 Angular Frontend                 │
│           (Customer Storefront + Admin)          │
└──────────────────────┬──────────────────────────┘
                       │
              ┌────────▼────────┐
              │   API Gateway   │
              │   (Port 8080)   │
              └────────┬────────┘
                       │
    ┌──────────────────┼──────────────────┐
    │                  │                  │
┌───▼───┐  ┌────▼────┐  ┌────▼────┐  ┌────▼────┐
│ User  │  │ Product │  │Inventory│  │  Order  │
│Service│  │ Service │  │ Service │  │ Service │
│ :8081 │  │  :8082  │  │  :8083  │  │  :8084  │
└───┬───┘  └────┬────┘  └────┬────┘  └────┬────┘
    │           │            │            │
    │           │            │       ┌────┴────┐
    │           │            │       │ Payment │
    │           │            │       │Service  │
    │           │            │       │  :8085  │
    │           │            │       └────┬────┘
    │           │            │            │
    │           │            │       ┌────▼────┐
    │           │            │       │ Notifi- │
    │           │            │       │cation   │
    │           │            │       │ :8086   │
    │           │            │       └─────────┘
    │           │            │
┌───▼───────────▼────────────▼────┐
│           MySQL Databases       │
│  (6 separate schemas)           │
└─────────────────────────────────┘

    ════════ Kafka Events ════════
    order-events  → Inventory, Payment, Notification
    inventory-events → Order, Notification
    payment-events → Order, Notification
```

## Microservices

| Service | Port | Database | Responsibilities |
|---------|------|----------|-----------------|
| API Gateway | 8080 | - | Routing, JWT validation, CORS |
| User Service | 8081 | ecommerce_user | Registration, Login, Profile |
| Product Service | 8082 | ecommerce_product | Products, Categories, Search |
| Inventory Service | 8083 | ecommerce_inventory | Stock management, Reservation |
| Order Service | 8084 | ecommerce_order | Orders, Cart, Checkout |
| Payment Service | 8085 | ecommerce_payment | Payment processing |
| Notification Service | 8086 | ecommerce_notification | Notifications |

## Kafka Architecture

### Topics

| Topic | Producer | Consumers | Purpose |
|-------|----------|-----------|---------|
| order-events | Order Service | Inventory, Payment, Notification | Order lifecycle events |
| inventory-events | Inventory Service | Order, Notification | Stock changes |
| payment-events | Payment Service | Order, Notification | Payment results |

### Event Types

| Event | Publisher | Consumers |
|-------|-----------|-----------|
| ORDER_CREATED | Order Service | Inventory, Payment, Notification |
| ORDER_CONFIRMED | Order Service | Notification |
| ORDER_SHIPPED | Order Service | Notification |
| ORDER_DELIVERED | Order Service | Notification |
| ORDER_CANCELLED | Order Service | Inventory, Notification |
| INVENTORY_RESERVED | Inventory Service | Order, Notification |
| INVENTORY_RELEASED | Inventory Service | Order, Notification |
| PAYMENT_COMPLETED | Payment Service | Order, Notification |
| PAYMENT_FAILED | Payment Service | Order, Notification |

## Order Lifecycle

```
PENDING → CONFIRMED → PROCESSING → SHIPPED → OUT_FOR_DELIVERY → DELIVERED

PENDING → CANCELLED
CONFIRMED → CANCELLED
PROCESSING → CANCELLED
```

## Payment Workflow

```
Checkout → Create Order → Reserve Inventory → Publish ORDER_CREATED
→ Process Payment → Payment Success? 
  → YES: PAYMENT_COMPLETED → Order CONFIRMED → Notification
  → NO:  PAYMENT_FAILED → Order FAILED → Release Inventory → Notification
```

## Database Design

Each microservice owns its own database:

- **ecommerce_user**: users
- **ecommerce_product**: products, categories
- **ecommerce_inventory**: inventory
- **ecommerce_order**: orders, order_items, carts, cart_items
- **ecommerce_payment**: payments
- **ecommerce_notification**: notifications

## API Documentation

Each service exposes Swagger UI:
- User Service: `http://localhost:8081/swagger-ui.html`
- Product Service: `http://localhost:8082/swagger-ui.html`
- Inventory Service: `http://localhost:8083/swagger-ui.html`
- Order Service: `http://localhost:8084/swagger-ui.html`
- Payment Service: `http://localhost:8085/swagger-ui.html`
- Notification Service: `http://localhost:8086/swagger-ui.html`

## Security

- JWT-based authentication with BCrypt password hashing
- Role-based authorization (CUSTOMER, ADMIN)
- API Gateway handles JWT validation
- Separate service URLs configured via environment variables
- No hardcoded credentials or secrets

## Docker Setup

### Prerequisites
- Docker
- Docker Compose

### Running with Docker

```bash
# Clone and start everything
git clone <repo-url>
cd ecommerce-system
docker-compose up --build
```

### Services in Docker

| Service | Container | Port |
|---------|-----------|------|
| MySQL | ecommerce-mysql | 3306 |
| Kafka | ecommerce-kafka | 9092 |
| Zookeeper | ecommerce-zookeeper | 2181 |
| API Gateway | ecommerce-api-gateway | 8080 |
| All microservices | ecommerce-*-service | 8081-8086 |

## Local Setup

### Prerequisites
- Java 17+
- Maven 3.9+
- Node.js 18+
- MySQL 8.0
- Apache Kafka

### Backend

```bash
# Start MySQL and Kafka first, then:
cd ecommerce-system

# Build all services
mvn clean install -DskipTests

# Run each service
cd user-service && mvn spring-boot:run
cd product-service && mvn spring-boot:run
cd inventory-service && mvn spring-boot:run
cd order-service && mvn spring-boot:run
cd payment-service && mvn spring-boot:run
cd notification-service && mvn spring-boot:run
cd api-gateway && mvn spring-boot:run
```

### Frontend

```bash
cd frontend
npm install
ng serve
```

Access at: `http://localhost:4200`

## Demo Credentials

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@example.com | password |
| Customer | customer1@example.com | password |
| Customer | customer2@example.com | password |

## Project Structure

```
ecommerce-system/
├── pom.xml                          # Parent POM
├── docker-compose.yml               # Docker orchestration
├── .env.example                     # Environment template
├── api-gateway/                     # Spring Cloud Gateway
├── user-service/                    # Authentication & User management
├── product-service/                 # Products & Categories
├── inventory-service/               # Stock management
├── order-service/                   # Orders & Cart
├── payment-service/                 # Payment processing
├── notification-service/            # Kafka event consumer → Notifications
├── frontend/                        # Angular 17 application
│   └── src/app/
│       ├── components/
│       │   ├── customer/            # Storefront components
│       │   ├── admin/               # Admin dashboard components
│       │   └── shared/              # Navbar, Footer
│       ├── services/                # Angular services
│       ├── guards/                  # Route guards
│       ├── interceptors/            # JWT interceptor
│       └── models/                  # TypeScript models
└── docs/                            # Documentation
```

## Interview Topics

### Why Microservices?
Clear service boundaries. Each service (User, Product, Order, Payment, Inventory, Notification) has a single responsibility, can be deployed independently, and uses its own database.

### Why Kafka?
Asynchronous event-driven communication for decoupled services. When an order is placed, multiple services need to react (Inventory reserves stock, Payment processes, Notification sends alerts). Kafka provides reliable, scalable, event-driven communication without tight coupling.

### Why REST for some operations?
Synchronous REST for operations requiring immediate responses: product search, cart operations, user authentication. These need real-time data and immediate feedback.

### How does checkout work?
1. Validate cart items
2. Create order with PENDING status
3. Reserve inventory
4. Process payment
5. On success: confirm order, publish events
6. On failure: release inventory, mark order as failed

### How are order states controlled?
State transitions are validated: PENDING→CONFIRMED→PROCESSING→SHIPPED→OUT_FOR_DELIVERY→DELIVERED. Invalid transitions throw OrderStateException.

## Future Enhancements

- Product reviews and ratings
- Wishlist functionality
- Coupon/discount system
- Email notifications via SMTP
- Payment gateway integration (Stripe/Razorpay)
- Order tracking with delivery partner integration
- Analytics and reporting dashboard
- Unit and integration tests
- CI/CD pipeline

## License

MIT License

---

Built as a portfolio project demonstrating Java Full Stack development with microservices architecture.

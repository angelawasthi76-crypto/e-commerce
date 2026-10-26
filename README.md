# E-Commerce Order Management System

A full-stack e-commerce application built using Java, Spring Boot, Angular, MySQL, and Docker. It uses a microservices architecture to manage products, users, shopping carts, orders, and payments.

## Tech Stack

- **Backend:** Java 17, Spring Boot, Spring Security, Spring Data JPA
- **Frontend:** Angular 17, TypeScript, HTML, CSS
- **Database:** MySQL 8.0
- **Messaging:** Apache Kafka
- **Authentication:** JWT
- **Deployment:** Docker, Docker Compose
- **Testing:** JUnit, Mockito
- **Build Tools:** Maven, npm

## Features

- User registration and login
- JWT-based authentication and authorization
- Product listing and management
- Shopping cart management
- Order creation and management
- Payment-related functionality
- RESTful API integration
- Microservices architecture
- MySQL database integration
- Docker-based application setup
- Backend unit testing

## Architecture

The application contains a frontend and backend services:

- **API Gateway:** Routes requests to backend services.
- **User Service:** Manages user accounts.
- **Product Service:** Manages product information.
- **Cart Service:** Handles shopping cart operations.
- **Order Service:** Manages orders.
- **Payment Service:** Handles payment operations.
- **Notification Service:** Manages notifications.
- **Angular Frontend:** Provides the user interface.

## Prerequisites

Install the following before running the project:

- Java 17
- Node.js and npm
- Angular CLI
- Docker Desktop
- Git

## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/angelawasthi76-crypto/e-commerce.git
cd e-commerce
```

### 2. Configure Environment Variables

Create a local `.env` file with the environment variables required by the application.

Do not commit `.env` or expose passwords, API keys, or JWT secrets.

### 3. Start Backend Services

From the project root, run:

```bash
docker compose up --build -d
```

Check the running containers:

```bash
docker compose ps
```

### 4. Start the Frontend

Open a new terminal and run:

```bash
cd frontend
npm install
ng serve
```

Open the application in your browser:

http://localhost:4200

## Testing

Run backend tests from the relevant Spring Boot service directory:

```bash
mvn test
```

## Stop the Application

From the project root, run:

```bash
docker compose down
```

This stops the containers without intentionally deleting database volumes.

## Author

**Angel Awasthi**

- GitHub: [angelawasthi76-crypto](https://github.com/angelawasthi76-crypto)
- Project Repository: [E-Commerce Order Management System](https://github.com/angelawasthi76-crypto/e-commerce)

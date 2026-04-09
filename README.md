# 🏥 Patient Management System (Microservices)

![Java](https://img.shields.io/badge/Java-17-blue)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen)
![Kafka](https://img.shields.io/badge/Kafka-Event%20Driven-orange)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![Architecture](https://img.shields.io/badge/Architecture-Microservices-purple)
![Status](https://img.shields.io/badge/Status-Active-success)

---

## 📌 Overview

A **production-style Patient Management System** built using **Microservices Architecture** with:

* REST APIs
* gRPC communication
* Kafka event-driven architecture
* API Gateway (Spring Cloud Gateway)
* JWT-based authentication
* Dockerized deployment

---

## 🧱 Architecture Diagram

```
                ┌────────────────────┐
                │     Frontend       │
                └─────────┬──────────┘
                          │
                          ▼
                ┌────────────────────┐
                │   API Gateway      │
                │ (Port: 4004)       │
                └──────┬───────┬─────┘
                       │       │
          ┌────────────┘       └────────────┐
          ▼                                 ▼
┌────────────────────┐           ┌────────────────────┐
│   Auth Service     │           │  Patient Service   │
│   (Port: 4005)     │           │   (Port: 4000)      │
└─────────┬──────────┘           └─────────┬──────────┘
          │                                │
          │ JWT                            │ gRPC
          ▼                                ▼
                              ┌────────────────────┐
                              │  Billing Service   │
                              │   (Port: 9001)     │
                              └────────────────────┘

                     ┌────────────────────┐
                     │       Kafka        │
                     │   (Event Broker)   │
                     └─────────┬──────────┘
                               ▼
                     ┌────────────────────┐
                     │ Analytics Service │
                     └────────────────────┘

                     ┌────────────────────┐
                     │   PostgreSQL DBs  │
                     └────────────────────┘
```

---

## 🧩 Microservices

### 🔹 API Gateway

* Entry point for all requests
* Routes traffic to services
* JWT validation (planned)

---

### 🔹 Auth Service

* Handles login
* Generates JWT tokens
* Connects to PostgreSQL

---

### 🔹 Patient Service

* CRUD operations
* Publishes Kafka events
* Calls billing-service via gRPC

---

### 🔹 Billing Service

* gRPC-based billing creation
* Future Kafka consumer

---

### 🔹 Analytics Service

* Kafka consumer
* Processes patient events

---

## ⚙️ Tech Stack

| Category         | Technology           |
| ---------------- | -------------------- |
| Language         | Java 17              |
| Framework        | Spring Boot          |
| Gateway          | Spring Cloud Gateway |
| Messaging        | Apache Kafka         |
| Communication    | REST + gRPC          |
| Database         | PostgreSQL           |
| Containerization | Docker               |
| Build Tool       | Maven                |

---

## 🐳 Docker Setup

### 🔑 Rule

```
❌ localhost (inside containers)
✅ container-name (Docker DNS)
```

---

## 🔌 Ports

| Service         | Port        |
| --------------- | ----------- |
| API Gateway     | 4004        |
| Patient Service | 4000        |
| Auth Service    | 4005        |
| Billing Service | 9001        |
| Kafka           | 9092 / 9094 |
| PostgreSQL      | 5000 / 5001 |

---

## 🚀 Run Locally (Docker)

### 1️⃣ Create Network

```bash
docker network create internal
```

---

### 2️⃣ Start Databases

```bash
docker run -d \
  --name patient-service-db \
  --network internal \
  -e POSTGRES_DB=db \
  -e POSTGRES_USER=admin_viewer \
  -e POSTGRES_PASSWORD=password \
  -p 5000:5432 postgres:15

docker run -d \
  --name auth-service-db \
  --network internal \
  -e POSTGRES_DB=db \
  -e POSTGRES_USER=admin_user \
  -e POSTGRES_PASSWORD=password \
  -p 5001:5432 postgres:15
```

---

### 3️⃣ Start Kafka

```bash
docker run -d \
  --name kafka \
  --network internal \
  -p 9092:9092 \
  -p 9094:9094 \
  apache/kafka
```

---

### 4️⃣ Build Services

```bash
docker build -t patient-service ./patient-service
docker build -t billing-service ./billing-service
docker build -t analytics-service ./analytics-service
docker build -t auth-service ./auth-service
docker build -t api-gateway ./api-gateway
```

---

### 5️⃣ Run Services

```bash
docker run -d --name patient-service --network internal -p 4000:4000 patient-service
docker run -d --name billing-service --network internal -p 9001:9001 billing-service
docker run -d --name analytics-service --network internal analytics-service
docker run -d --name auth-service --network internal -p 4005:4005 auth-service
docker run -d --name api-gateway --network internal -p 4004:4004 api-gateway
```

---

## 🔐 Authentication Flow

1. Login:

```
POST /auth/login
```

2. Receive JWT:

```
Authorization: Bearer <token>
```

3. Access APIs via Gateway

---

## 📡 API Endpoints

### Auth

```
POST /auth/login
```

### Patient

```
GET    /api/patients
POST   /api/patients
PUT    /api/patients/{id}
DELETE /api/patients/{id}
```

### Analytics

```
GET /api/analytics
```

---

## 🧠 Key Learnings

* Microservices design (sync + async)
* Docker networking
* Kafka event-driven systems
* API Gateway routing
* JWT-based auth
* Debugging distributed systems

---

## ⚠️ Common Issues

| Issue                | Cause                  |
| -------------------- | ---------------------- |
| Connection refused   | Service not running    |
| UnknownHostException | Wrong network          |
| Auth failed          | Wrong DB credentials   |
| Kafka issues         | Wrong bootstrap server |

---

## 👨‍💻 Author

Built as a **production-style backend system** to simulate real-world distributed architecture.

---

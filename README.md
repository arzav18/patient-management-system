# Patient Management System (Microservices)

## 🚀 Overview

This project is a **Production-Ready Patient Management System** built using a microservices architecture.

It demonstrates:

* REST APIs (Spring Boot)
* gRPC communication
* Docker-based deployment
* PostgreSQL integration

---

## 🏗️ Architecture

```
Client → patient-service (REST)
        → PostgreSQL
        → billing-service (gRPC)
```

---

## ⚙️ Tech Stack

* Java 17 / 21
* Spring Boot
* Spring Data JPA
* PostgreSQL
* Docker
* gRPC
* Protobuf
* Maven

---

## 📦 Services

### 1. patient-service

* Port: 4000
* Features:

  * Create, update, delete patients
  * Stores data in PostgreSQL
  * Calls billing-service via gRPC

---

### 2. billing-service

* Ports:

  * 4001 (HTTP)
  * 9001 (gRPC)
* Features:

  * Creates billing account via gRPC

---

### 3. PostgreSQL

* Port: 5000
* Used for patient data storage

---

## 🐳 Docker Setup

### Create Network

```bash
docker network create patient-network
```

---

### Run PostgreSQL

```bash
docker run -d \
--name patient-service-db \
--network patient-network \
-p 5000:5432 \
-e POSTGRES_DB=db \
-e POSTGRES_USER=admin_viewer \
-e POSTGRES_PASSWORD=password \
postgres:15
```

---

### Build Services

```bash
cd patient-service
./mvnw clean package -DskipTests
docker build -t patient-service .

cd ../billing-service
./mvnw clean package -DskipTests
docker build -t billing-service .
```

---

### Run Services

```bash
docker run -d \
--name billing-service \
--network patient-network \
-p 4001:4001 \
-p 9001:9001 \
billing-service

docker run -d \
--name patient-service \
--network patient-network \
-p 4000:4000 \
patient-service
```

---

## 🔗 gRPC Configuration

In `patient-service`:

```properties
billing.service.address=billing-service
billing.service.grpc.port=9001
```

---

## 📌 Important Notes

### ❗ Docker Networking

* Use service name for communication
* Do NOT use `localhost`

---

### ❗ Build Strategy

* JAR is built locally using Maven
* Docker only runs the JAR

---

## 🧪 API Example

### Create Patient

```http
POST http://localhost:4000/patients
```

---

## ✅ Current Status

* REST APIs working
* PostgreSQL connected
* gRPC communication working
* Docker setup complete

---

## 🔮 Future Enhancements

* Docker Compose
* API Gateway
* Service Discovery
* Circuit Breaker
* Centralized Logging
* Kubernetes deployment

---

## 👨‍💻 Author

Arzav Jain

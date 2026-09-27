Spring Cloud Gateway Redis Rate Limiter

A reference implementation of API request rate limiting using Spring Cloud Gateway, Spring WebFlux, and Redis. The gateway sits in front of a Spring Boot backend service and throttles incoming traffic per client IP using a token-bucket algorithm backed by Redis.

<p align="center"> <img alt="Java" src="https://img.shields.io/badge/Java-25-orange"> <img alt="Spring Boot" src="https://img.shields.io/badge/Spring%20Boot-green"> <img alt="Spring Cloud Gateway" src="https://img.shields.io/badge/Spring%20Cloud%20Gateway-6DB33F"> <img alt="Redis" src="https://img.shields.io/badge/Redis-DC382D"> <img alt="License" src="https://img.shields.io/badge/License-MIT-blue"> </p>
✨ Features
🚦 API request rate limiting
⚡ Redis-based rate-limit state management
🌐 Spring Cloud Gateway routing
🔑 IP-based client identification
🪣 Token-bucket rate limiting
🐳 Redis running inside Docker
🔄 Reactive Gateway using Spring WebFlux
🚫 HTTP 429 Too Many Requests handling
🔗 Spring Boot REST backend integration
⚙️ Rate Limiting Configuration
Configuration	Value
Replenish Rate	20 requests/sec
Burst Capacity	40 requests
Requested Tokens	1/request
Client Key	IP Address
Gateway Port	8082
Backend Port	8080
Redis Port	6379
How it works
Request
   ↓
Gateway
   ↓
Identify Client IP
   ↓
Check Redis Token Bucket
   ↓
 ┌───────────────┐
 │ Token exists? │
 └───────┬───────┘
       Yes │ No
          │  │
          ▼  ▼
       Backend  HTTP 429
🛠️ Technologies Used
Technology	Purpose
☕ Java 25	Programming Language
🌱 Spring Boot	Backend Framework
🌐 Spring Cloud Gateway	API Gateway & Routing
⚡ Spring WebFlux	Reactive Programming
🔴 Redis	Rate Limit State Management
🐳 Docker	Redis Containerization
📦 Maven	Dependency & Build Management
🔧 Git	Version Control
🐙 GitHub	Source Code Management
📂 Project Structure
spring-cloud-gateway-redis-rate-limiter/
│
├── hello-backend/
│   └── hello-backend/
│       ├── src/
│       ├── pom.xml
│       └── ...
│
├── rate-limiter-example/
│   └── rate-limiter-example/
│       ├── src/
│       │   └── main/
│       │       ├── java/
│       │       │   └── com.deep.rate_limiter/
│       │       │       ├── RateLimiterExampleApplication.java
│       │       │       └── config/
│       │       │           └── RateLimiterConfig.java
│       │       │
│       │       └── resources/
│       │           └── application.yml
│       │
│       └── pom.xml
│
└── README.md
🚀 Getting Started
1️⃣ Clone the Repository
git clone https://github.com/deepchakraborty71/spring-cloud-gateway-redis-rate-limiter.git
cd spring-cloud-gateway-redis-rate-limiter
2️⃣ Start Redis with Docker

Make sure Docker Desktop is running.

docker run --name redis-rate-limiter -p 6379:6379 -d redis

Check the container:

docker ps

Test Redis:

docker exec -it redis-rate-limiter redis-cli ping

Expected:

PONG
3️⃣ Start the Backend

Run:

HelloBackendApplication

Backend runs on:

http://localhost:8080

Test:

http://localhost:8080/hello

Expected response:

Hello from Backend Service
4️⃣ Start the Gateway

Run:

RateLimiterExampleApplication

Gateway runs on:

http://localhost:8082

Test:

http://localhost:8082/hello

Expected response:

Hello from Backend Service
🧪 Testing Rate Limiting

Send multiple requests to the Gateway:

curl http://localhost:8082/hello

For Windows CMD:

for /L %i in (1,1,100) do @curl -s -o nul -w "%{http_code}\n" http://localhost:8082/hello

Successful requests:

200

When the rate limit is exceeded:

429
<p align="center">⭐ If you find this project useful, consider giving it a star!</p>

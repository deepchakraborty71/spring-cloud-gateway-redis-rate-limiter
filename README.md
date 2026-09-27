Spring Cloud Gateway Redis Rate Limiter

A reference implementation of API request rate limiting using Spring Cloud Gateway, Spring WebFlux, and Redis. The gateway sits in front of a Spring Boot backend service and throttles incoming traffic per client IP using a token-bucket algorithm backed by Redis.

<p align="center"> <img alt="Java" src="https://img.shields.io/badge/Java-25-orange"> <img alt="Spring Boot" src="https://img.shields.io/badge/Spring%20Boot-green"> <img alt="Spring Cloud Gateway" src="https://img.shields.io/badge/Spring%20Cloud%20Gateway-6DB33F"> <img alt="Redis" src="https://img.shields.io/badge/Redis-DC382D"> <img alt="License" src="https://img.shields.io/badge/License-MIT-blue"> </p>
Table of Contents
Architecture
Features
Rate Limiting Configuration
How It Works
Technologies Used
Project Structure
Getting Started
Testing Rate Limiting
Gateway Configuration
Key Resolver
Request Flow
Key Concepts Demonstrated
Future Improvements
Author
Architecture
HTTP Request
Rate Limit Check
Request Allowed
ClientBrowser / API
Spring Cloud Gateway:8082
Redis:6379
Spring Boot Backend:8080
Hello from Backend Service
Features
🚦 API request rate limiting
⚡ Redis-based rate-limit state management
🌐 Spring Cloud Gateway routing
🔑 IP-based client identification
🪣 Token-bucket rate limiting algorithm
🐳 Redis running inside Docker
🔄 Reactive gateway built on Spring WebFlux
🚫 HTTP 429 (Too Many Requests) handling
🔗 Integration with a Spring Boot REST backend
Rate Limiting Configuration
Configuration	Value
Replenish Rate	20 requests/sec
Burst Capacity	40 requests
Requested Tokens	1 per request
Client Key	IP address
Gateway Port	8082
Backend Port	8080
Redis Port	6379
How It Works
Yes
No
Incoming Request
Gateway identifies client IP
Check Redis token bucket
Token available?
Forward to Backend
Return HTTP 429
Technologies Used
Technology	Purpose
☕ Java 25	Programming language
🌱 Spring Boot	Backend framework
🌐 Spring Cloud Gateway	API gateway and routing
⚡ Spring WebFlux	Reactive programming
🔴 Redis	Rate-limit state management
🐳 Docker	Redis containerization
📦 Maven	Dependency and build management
🔧 Git	Version control
🐙 GitHub	Source code management
Project Structure
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
Getting Started
1. Clone the repository
bash
git clone https://github.com/deepchakraborty71/spring-cloud-gateway-redis-rate-limiter.git
cd spring-cloud-gateway-redis-rate-limiter
2. Start Redis with Docker

Make sure Docker Desktop is running, then start a Redis container:

bash
docker run --name redis-rate-limiter -p 6379:6379 -d redis

Verify the container is running:

bash
docker ps

Test the Redis connection:

bash
docker exec -it redis-rate-limiter redis-cli ping

Expected output:

PONG
3. Start the backend

Run the HelloBackendApplication class.

The backend starts on http://localhost:8080.

Test it:

http://localhost:8080/hello

Expected response:

Hello from Backend Service
4. Start the gateway

Run the RateLimiterExampleApplication class.

The gateway starts on http://localhost:8082.

Test it:

http://localhost:8082/hello

Expected response:

Hello from Backend Service
Testing Rate Limiting

Send repeated requests through the gateway:

bash
curl http://localhost:8082/hello

On Windows (Command Prompt), send 100 requests in a loop and print each status code:

cmd
for /L %i in (1,1,100) do @curl -s -o nul -w "%{http_code}\n" http://localhost:8082/hello
Requests within the allowed rate return 200
Requests that exceed the configured limit return 429
Gateway Configuration
yaml
server:
  port: 8082

spring:
  application:
    name: ratelimiter-example

  data:
    redis:
      host: localhost
      port: 6379

  cloud:
    gateway:
      server:
        webflux:
          routes:
            - id: route1
              uri: http://localhost:8080
              predicates:
                - Path=/hello
              filters:
                - name: RequestRateLimiter
                  args:
                    redis-rate-limiter.replenishRate: 20
                    redis-rate-limiter.burstCapacity: 40
                    redis-rate-limiter.requestedTokens: 1
                    key-resolver: "#{@ipKeyResolver}"
Key Resolver

The gateway uses the client's IP address as the rate-limiting key:

java
@Bean
public KeyResolver ipKeyResolver() {
    return exchange -> {
        String ip = exchange.getRequest()
                .getRemoteAddress()
                .getAddress()
                .getHostAddress();

        return Mono.just(ip);
    };
}

This allows the gateway to apply independent rate limits to each client IP address.

Request Flow

Allowed request

Token available
Client
Gateway :8082
Redis rate limiter
Backend :8080
HTTP 200

Rate-limited request

No token available
Client
Gateway :8082
Redis rate limiter
HTTP 429 Too ManyRequests
Key Concepts Demonstrated
API Gateway patterns
Microservice communication
Rate limiting and the token bucket algorithm
Redis as a distributed state store
Reactive programming with Spring WebFlux
REST API design
Docker containerization
Request routing and client identification
HTTP status code handling
Future Improvements
🔐 Add JWT authentication
👤 Implement user-based rate limiting
🔑 Add API-key-based rate limiting
📊 Add monitoring with Spring Boot Actuator
📈 Add metrics and dashboards
🐳 Create a Docker Compose setup for all services
🧪 Add unit and integration tests
🔄 Add a circuit breaker
☁️ Deploy the system to AWS
Author

Deep Chakraborty 🎓 B.Tech, Information Technology 💻 Java | Spring Boot | Microservices | AI/ML

<p align="center">⭐ If you find this project useful, consider giving it a star!</p>

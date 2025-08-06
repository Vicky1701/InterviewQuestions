# Spring Microservices - Interview Questions & Answers

## 🎯 **Essential Questions for 2+ Years Experience**

---

## **1. What are Microservices? How are they different from Monolithic architecture?**

**Answer:**

**Microservices** is an architectural pattern where an application is built as a collection of small, independent services that communicate over well-defined APIs.

### **Comparison Table:**

| Aspect | Monolithic | Microservices |
|--------|------------|---------------|
| **Deployment** | Single deployable unit | Multiple independent deployments |
| **Scalability** | Scale entire application | Scale individual services |
| **Technology Stack** | Single technology stack | Multiple technology stacks |
| **Team Structure** | Large teams, shared codebase | Small autonomous teams |
| **Database** | Shared database | Database per service |
| **Fault Tolerance** | Single point of failure | Isolated failures |
| **Development Speed** | Slower for large teams | Faster parallel development |
| **Complexity** | Simple deployment, complex codebase | Complex deployment, simple services |

### **Monolithic Example:**
```java
@SpringBootApplication
public class ECommerceApplication {
    // Single application containing:
    // - User Management
    // - Product Catalog
    // - Order Processing
    // - Payment Processing
    // - Inventory Management
    // - Notification Service
}

@Controller
public class OrderController {
    
    @Autowired
    private UserService userService;           // All services in same application
    
    @Autowired
    private ProductService productService;
    
    @Autowired
    private PaymentService paymentService;
    
    @Autowired
    private InventoryService inventoryService;
    
    @PostMapping("/orders")
    public ResponseEntity<Order> createOrder(@RequestBody OrderRequest request) {
        // All operations in single transaction
        User user = userService.getUser(request.getUserId());
        Product product = productService.getProduct(request.getProductId());
        inventoryService.checkStock(request.getProductId(), request.getQuantity());
        Payment payment = paymentService.processPayment(request.getPaymentDetails());
        
        Order order = orderService.createOrder(request);
        return ResponseEntity.ok(order);
    }
}
```

### **Microservices Example:**
```java
// User Service
@SpringBootApplication
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/{userId}")
    public ResponseEntity<User> getUser(@PathVariable Long userId) {
        User user = userService.findById(userId);
        return ResponseEntity.ok(user);
    }
}

// Order Service (separate application)
@SpringBootApplication
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}

@RestController
@RequestMapping("/api/orders")
public class OrderController {
    
    @Autowired
    private OrderService orderService;
    
    @Autowired
    private UserServiceClient userServiceClient;    // HTTP client
    
    @Autowired
    private PaymentServiceClient paymentServiceClient;
    
    @PostMapping
    public ResponseEntity<Order> createOrder(@RequestBody OrderRequest request) {
        // Inter-service communication via HTTP
        User user = userServiceClient.getUser(request.getUserId());
        PaymentResult payment = paymentServiceClient.processPayment(request.getPaymentDetails());
        
        Order order = orderService.createOrder(request);
        return ResponseEntity.ok(order);
    }
}
```

---

## **2. What is Service Discovery? How do you implement it in Spring Cloud?**

**Answer:**

**Service Discovery** is a mechanism that allows services to find and communicate with each other without hardcoding network locations.

### **Why Service Discovery is Needed:**
- Dynamic IP addresses in cloud environments
- Auto-scaling changes service instances
- Load balancing across multiple instances
- Fault tolerance when instances fail

### **Implementation with Netflix Eureka:**

#### **1. Eureka Server Setup:**
```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

```yaml
# eureka-server application.yml
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false    # Don't register itself
    fetch-registry: false          # Don't fetch registry
    service-url:
      default-zone: http://localhost:8761/eureka/
```

#### **2. Service Registration (Eureka Client):**
```java
@SpringBootApplication
@EnableEurekaClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```

```yaml
# user-service application.yml
server:
  port: 8081

spring:
  application:
    name: user-service

eureka:
  client:
    service-url:
      default-zone: http://localhost:8761/eureka/
  instance:
    prefer-ip-address: true
    lease-renewal-interval-in-seconds: 10
    lease-expiration-duration-in-seconds: 30
```

#### **3. Service Discovery and Communication:**
```java
@RestController
public class OrderController {
    
    @Autowired
    private DiscoveryClient discoveryClient;
    
    @Autowired
    private RestTemplate restTemplate;
    
    @Autowired
    private LoadBalancerClient loadBalancer;
    
    // Method 1: Using DiscoveryClient
    @GetMapping("/orders/{orderId}")
    public Order getOrderWithUser(Long orderId) {
        // Discover user service instances
        List<ServiceInstance> instances = discoveryClient.getInstances("user-service");
        if (!instances.isEmpty()) {
            ServiceInstance instance = instances.get(0);
            String userServiceUrl = instance.getUri().toString();
            
            // Make HTTP call to user service
            User user = restTemplate.getForObject(
                userServiceUrl + "/api/users/" + orderId, 
                User.class
            );
        }
        return order;
    }
    
    // Method 2: Using LoadBalancerClient
    @GetMapping("/orders/{orderId}/user")
    public User getUserForOrder(@PathVariable Long orderId) {
        ServiceInstance instance = loadBalancer.choose("user-service");
        String userServiceUrl = instance.getUri().toString();
        
        return restTemplate.getForObject(
            userServiceUrl + "/api/users/" + orderId, 
            User.class
        );
    }
}

@Configuration
public class RestTemplateConfig {
    
    @Bean
    @LoadBalanced  // Enable load balancing
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

// Method 3: Using Load-balanced RestTemplate
@RestController
public class OrderController {
    
    @Autowired
    @LoadBalanced
    private RestTemplate restTemplate;
    
    @GetMapping("/orders/{orderId}")
    public Order getOrder(@PathVariable Long orderId) {
        // Service name instead of IP:PORT
        User user = restTemplate.getForObject(
            "http://user-service/api/users/" + orderId, 
            User.class
        );
        
        return orderService.getOrderWithUser(orderId, user);
    }
}
```

### **Alternative: Spring Cloud Consul**
```yaml
# application.yml for Consul
spring:
  application:
    name: user-service
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        enabled: true
        service-name: user-service
        health-check-path: /actuator/health
        health-check-interval: 10s
```

---

## **3. What is API Gateway? How do you implement it with Spring Cloud Gateway?**

**Answer:**

**API Gateway** acts as a single entry point for all client requests, providing routing, load balancing, authentication, and other cross-cutting concerns.

### **Benefits of API Gateway:**
- Single entry point for all requests
- Request routing and load balancing
- Authentication and authorization
- Rate limiting and throttling
- Request/response transformation
- Monitoring and analytics

### **Spring Cloud Gateway Implementation:**

#### **1. Gateway Setup:**
```java
@SpringBootApplication
public class ApiGatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
```

#### **2. Route Configuration:**
```yaml
# application.yml
server:
  port: 8080

spring:
  application:
    name: api-gateway
  cloud:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service          # Load balanced URI
          predicates:
            - Path=/api/users/**
          filters:
            - StripPrefix=1               # Remove /api prefix
            
        - id: order-service
          uri: lb://order-service
          predicates:
            - Path=/api/orders/**
          filters:
            - StripPrefix=1
            - AddRequestHeader=X-Gateway, Spring-Cloud-Gateway
            
        - id: product-service
          uri: lb://product-service
          predicates:
            - Path=/api/products/**
            - Method=GET,POST
          filters:
            - StripPrefix=1
            - RewritePath=/api/products/(?<segment>.*), /products/${segment}

eureka:
  client:
    service-url:
      default-zone: http://localhost:8761/eureka/
```

#### **3. Programmatic Route Configuration:**
```java
@Configuration
public class GatewayConfig {
    
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
            .route("user-service", r -> r.path("/api/users/**")
                .filters(f -> f
                    .stripPrefix(1)
                    .addRequestHeader("X-Gateway", "Spring-Cloud-Gateway")
                    .retry(config -> config.setRetries(3))
                )
                .uri("lb://user-service")
            )
            .route("order-service", r -> r.path("/api/orders/**")
                .and()
                .method(HttpMethod.GET, HttpMethod.POST)
                .filters(f -> f
                    .stripPrefix(1)
                    .circuitBreaker(config -> config
                        .setName("order-service-cb")
                        .setFallbackUri("forward:/fallback/orders")
                    )
                )
                .uri("lb://order-service")
            )
            .build();
    }
}
```

#### **4. Custom Filters:**
```java
@Component
public class AuthenticationFilter implements GatewayFilter {
    
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        
        if (!request.getHeaders().containsKey("Authorization")) {
            ServerHttpResponse response = exchange.getResponse();
            response.setStatusCode(HttpStatus.UNAUTHORIZED);
            return response.setComplete();
        }
        
        return chain.filter(exchange);
    }
}

@Component
public class LoggingFilter implements GlobalFilter {
    
    private static final Logger logger = LoggerFactory.getLogger(LoggingFilter.class);
    
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        
        logger.info("Request: {} {}", request.getMethod(), request.getPath());
        
        return chain.filter(exchange).then(Mono.fromRunnable(() -> {
            ServerHttpResponse response = exchange.getResponse();
            logger.info("Response: {}", response.getStatusCode());
        }));
    }
}

@Configuration
public class FilterConfig {
    
    @Bean
    public FilterRegistrationBean<AuthenticationFilter> authFilter() {
        FilterRegistrationBean<AuthenticationFilter> registrationBean = new FilterRegistrationBean<>();
        registrationBean.setFilter(new AuthenticationFilter());
        registrationBean.addUrlPatterns("/api/secured/*");
        return registrationBean;
    }
}
```

#### **5. Rate Limiting:**
```java
@Configuration
public class RateLimitConfig {
    
    @Bean
    public RedisRateLimiter redisRateLimiter() {
        return new RedisRateLimiter(10, 20); // 10 requests per second, burst of 20
    }
    
    @Bean
    public KeyResolver userKeyResolver() {
        return exchange -> exchange.getRequest()
            .getHeaders()
            .getFirst("X-User-ID") != null 
                ? Mono.just(exchange.getRequest().getHeaders().getFirst("X-User-ID"))
                : Mono.just("anonymous");
    }
}
```

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: rate-limited-service
          uri: lb://user-service
          predicates:
            - Path=/api/users/**
          filters:
            - name: RequestRateLimiter
              args:
                redis-rate-limiter.replenishRate: 10
                redis-rate-limiter.burstCapacity: 20
                key-resolver: "#{@userKeyResolver}"
```

---

## **4. What is Circuit Breaker pattern? How do you implement it with Resilience4j?**

**Answer:**

**Circuit Breaker** prevents cascading failures by monitoring service calls and "opening" the circuit when failure rate exceeds threshold.

### **Circuit Breaker States:**
1. **CLOSED** - Normal operation, requests pass through
2. **OPEN** - Circuit is open, requests fail fast
3. **HALF_OPEN** - Limited requests allowed to test service recovery

### **Implementation with Resilience4j:**

#### **1. Dependencies:**
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>
```

#### **2. Configuration:**
```yaml
# application.yml
resilience4j:
  circuitbreaker:
    instances:
      user-service:
        sliding-window-size: 10
        minimum-number-of-calls: 5
        failure-rate-threshold: 50
        wait-duration-in-open-state: 30s
        permitted-number-of-calls-in-half-open-state: 3
        automatic-transition-from-open-to-half-open-enabled: true
        
      payment-service:
        sliding-window-size: 20
        failure-rate-threshold: 60
        wait-duration-in-open-state: 60s
        
  retry:
    instances:
      user-service:
        max-attempts: 3
        wait-duration: 1s
        retry-exceptions:
          - java.net.ConnectException
          - java.net.SocketTimeoutException
          
  timelimiter:
    instances:
      user-service:
        timeout-duration: 2s
```

#### **3. Using Circuit Breaker in Service:**
```java
@Service
public class OrderService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    @Autowired
    private CircuitBreakerFactory circuitBreakerFactory;
    
    // Method 1: Using CircuitBreakerFactory
    public User getUserById(Long userId) {
        CircuitBreaker circuitBreaker = circuitBreakerFactory.create("user-service");
        
        return circuitBreaker.executeSupplier(() -> {
            return restTemplate.getForObject(
                "http://user-service/api/users/" + userId, 
                User.class
            );
        });
    }
    
    // Method 2: With Fallback
    public User getUserWithFallback(Long userId) {
        CircuitBreaker circuitBreaker = circuitBreakerFactory.create("user-service");
        
        Supplier<User> userSupplier = CircuitBreaker.decorateSupplier(circuitBreaker, () -> {
            return restTemplate.getForObject(
                "http://user-service/api/users/" + userId, 
                User.class
            );
        });
        
        return Try.ofSupplier(userSupplier)
            .recover(throwable -> {
                // Fallback user
                return User.builder()
                    .id(userId)
                    .name("Unknown User")
                    .email("unknown@example.com")
                    .build();
            })
            .get();
    }
}
```

#### **4. Using Annotations:**
```java
@Service
public class UserServiceClient {
    
    @Autowired
    private RestTemplate restTemplate;
    
    @CircuitBreaker(name = "user-service", fallbackMethod = "getUserFallback")
    @Retry(name = "user-service")
    @TimeLimiter(name = "user-service")
    public CompletableFuture<User> getUser(Long userId) {
        return CompletableFuture.supplyAsync(() -> {
            return restTemplate.getForObject(
                "http://user-service/api/users/" + userId, 
                User.class
            );
        });
    }
    
    public CompletableFuture<User> getUserFallback(Long userId, Exception ex) {
        return CompletableFuture.completedFuture(
            User.builder()
                .id(userId)
                .name("Fallback User")
                .email("fallback@example.com")
                .build()
        );
    }
    
    @CircuitBreaker(name = "payment-service", fallbackMethod = "processPaymentFallback")
    public PaymentResult processPayment(PaymentRequest request) {
        return restTemplate.postForObject(
            "http://payment-service/api/payments", 
            request, 
            PaymentResult.class
        );
    }
    
    public PaymentResult processPaymentFallback(PaymentRequest request, Exception ex) {
        return PaymentResult.builder()
            .status("FAILED")
            .message("Payment service unavailable")
            .build();
    }
}
```

#### **5. Monitoring Circuit Breaker:**
```java
@RestController
@RequestMapping("/actuator")
public class CircuitBreakerController {
    
    @Autowired
    private CircuitBreakerRegistry circuitBreakerRegistry;
    
    @GetMapping("/circuitbreakers")
    public Map<String, String> getCircuitBreakerStates() {
        return circuitBreakerRegistry.getAllCircuitBreakers()
            .asJava()
            .stream()
            .collect(Collectors.toMap(
                CircuitBreaker::getName,
                cb -> cb.getState().toString()
            ));
    }
    
    @GetMapping("/circuitbreakers/{name}")
    public CircuitBreakerDetails getCircuitBreakerDetails(@PathVariable String name) {
        CircuitBreaker circuitBreaker = circuitBreakerRegistry.circuitBreaker(name);
        CircuitBreaker.Metrics metrics = circuitBreaker.getMetrics();
        
        return CircuitBreakerDetails.builder()
            .name(name)
            .state(circuitBreaker.getState().toString())
            .failureRate(metrics.getFailureRate())
            .numberOfCalls(metrics.getNumberOfCalls())
            .numberOfSuccessfulCalls(metrics.getNumberOfSuccessfulCalls())
            .numberOfFailedCalls(metrics.getNumberOfFailedCalls())
            .build();
    }
}
```

---

## **5. How do you handle distributed tracing in microservices?**

**Answer:**

**Distributed Tracing** helps track requests across multiple microservices to understand performance bottlenecks and debug issues.

### **Implementation with Spring Cloud Sleuth and Zipkin:**

#### **1. Dependencies:**
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
```

#### **2. Configuration:**
```yaml
# application.yml
spring:
  application:
    name: order-service
  sleuth:
    sampler:
      probability: 1.0    # Sample 100% of requests (reduce in production)
    zipkin:
      base-url: http://localhost:9411
      sender:
        type: web
  
logging:
  pattern:
    level: "%5p [${spring.application.name:},%X{traceId:-},%X{spanId:-}]"
```

#### **3. Custom Spans:**
```java
@Service
public class OrderService {
    
    @Autowired
    private Tracer tracer;
    
    @Autowired
    private UserServiceClient userServiceClient;
    
    @NewSpan("order-processing")
    public Order processOrder(OrderRequest request) {
        Span span = tracer.nextSpan()
            .name("validate-order")
            .tag("order.id", request.getOrderId().toString())
            .tag("user.id", request.getUserId().toString())
            .start();
        
        try (Tracer.SpanInScope ws = tracer.withSpanInScope(span)) {
            // Validation logic
            validateOrder(request);
            
            // This call will be automatically traced
            User user = userServiceClient.getUser(request.getUserId());
            
            span.tag("user.name", user.getName());
            
            Order order = createOrder(request, user);
            span.tag("order.status", order.getStatus());
            
            return order;
        } finally {
            span.end();
        }
    }
    
    @NewSpan("order-validation")
    public void validateOrder(OrderRequest request) {
        // Validation logic with automatic span creation
        if (request.getAmount().compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("Order amount must be positive");
        }
    }
}
```

#### **4. Custom Tracing for External Calls:**
```java
@Service
public class PaymentServiceClient {
    
    @Autowired
    private RestTemplate restTemplate;
    
    @Autowired
    private Tracer tracer;
    
    public PaymentResult processPayment(PaymentRequest request) {
        Span span = tracer.nextSpan()
            .name("payment-service-call")
            .tag("service.name", "payment-service")
            .tag("payment.amount", request.getAmount().toString())
            .start();
        
        try (Tracer.SpanInScope ws = tracer.withSpanInScope(span)) {
            PaymentResult result = restTemplate.postForObject(
                "http://payment-service/api/payments",
                request,
                PaymentResult.class
            );
            
            span.tag("payment.status", result.getStatus());
            return result;
            
        } catch (Exception e) {
            span.tag("error", e.getMessage());
            throw e;
        } finally {
            span.end();
        }
    }
}
```

#### **5. Baggage for Context Propagation:**
```java
@Service
public class OrderService {
    
    @Autowired
    private BaggageField userIdField;
    
    @Autowired
    private BaggageField correlationIdField;
    
    public Order processOrder(OrderRequest request) {
        // Set baggage that will be propagated to all downstream services
        userIdField.set(request.getUserId().toString());
        correlationIdField.set(UUID.randomUUID().toString());
        
        // All downstream calls will have this context
        return orderProcessingService.process(request);
    }
}

@Component
public class BaggageConfiguration {
    
    @Bean
    public BaggageField userIdField() {
        return BaggageField.create("user.id");
    }
    
    @Bean
    public BaggageField correlationIdField() {
        return BaggageField.create("correlation.id");
    }
}
```

---

## **🎯 Quick Review Questions**

1. **What is the difference between API Gateway and Load Balancer?**
   - API Gateway: Application-level routing with features like auth, rate limiting
   - Load Balancer: Network-level traffic distribution

2. **How do you handle data consistency in microservices?**
   - Saga pattern, Event sourcing, CQRS, eventual consistency

3. **What is Service Mesh and how is it different from API Gateway?**
   - Service Mesh: Infrastructure layer for service-to-service communication
   - API Gateway: Entry point for external clients

4. **How do you implement configuration management in microservices?**
   - Spring Cloud Config Server, environment variables, ConfigMaps

5. **What is the Two-Phase Commit problem in microservices?**
   - Distributed transaction complexity; use Saga pattern instead

---

**🚀 Pro Tips for Interview Success:**
- Understand the trade-offs between monolith and microservices
- Know when NOT to use microservices
- Practice designing service boundaries
- Understand eventual consistency concepts
- Be familiar with microservice patterns (Circuit Breaker, Saga, CQRS)

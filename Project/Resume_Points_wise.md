# Full Stack Developer Interview Preparation Guide

## Point 1: Microservices Architecture

### Questions to Prepare:
- What is a microservice?
- What microservices did you build in your project?
- How do microservices communicate with each other?
- What's the difference between B2B and B2C?
- How did you deploy your microservices?
- What are the advantages and disadvantages of microservices?
- How do you handle transactions across multiple microservices?
- How did you handle communication failures between microservices?
- What is service discovery? Did you use it?
- How do you maintain data consistency across microservices?
- What design patterns did you use in microservices?
- How do you handle versioning in microservices?

---

## Point 2: Database Indexing & Query Optimization

### Questions to Prepare:
- What is a database index?
- What is a composite index?
- Which tables did you add indexes on?
- How do you find slow queries?
- Give an example of a slow query you optimized
- How do you identify which queries need optimization?
- Explain the EXPLAIN command output
- When should you NOT use an index?
- What is the difference between clustered and non-clustered index?
- How does composite index work? Give an example
- What other optimization techniques did you use besides indexing?
- How do you handle database connection pooling?

---

## Point 3: Redis Caching

### Questions to Prepare:
- What is Redis?
- What is TTL?
- What data did you cache in Redis?
- Why use Redis instead of database?
- What happens when cache expires?
- What Redis data types did you use?
- What caching strategies did you use?
- How do you handle cache invalidation?
- What happens if Redis goes down?
- How did you decide TTL values?
- What is cache stampede? How to prevent it?
- Redis vs Memcached - why Redis?
- How do you ensure cache and database are in sync?

---

## Point 4: Kafka Implementation

### Questions to Prepare:
- What is Kafka?
- What is a Kafka producer?
- What is a Kafka consumer?
- What is a Kafka topic?
- What topics did you work with?
- What is a dead-letter queue (DLQ)?
- Why did you use Kafka?
- Explain Kafka architecture components
- What is a partition in Kafka? Why is it important?
- How did you implement retry mechanism?
- What is offset in Kafka? How does it work?
- How do you ensure message ordering in Kafka?
- What is consumer group and consumer rebalancing?
- How did you handle idempotency (duplicate messages)?
- How do you handle error scenarios in Kafka consumers?
- What is Kafka broker?

---

## Point 5: REST API Development

### Questions to Prepare:
- What is a REST API?
- What are HTTP methods you used?
- Give examples of APIs you created
- What is OpenAPI/Swagger?
- What does @RestController do?
- What is @RequestMapping or @GetMapping?
- What are REST principles/constraints?
- How do you handle exceptions in Spring Boot REST APIs?
- What HTTP status codes did you use?
- How do you implement pagination in REST APIs?
- How do you version your APIs?
- What is the difference between PUT and PATCH?
- How do you handle file uploads in Spring Boot?
- How do you validate request data?
- What is @RequestBody and @ResponseBody?

---

## Point 6: Keycloak & Security

### Questions to Prepare:
- What is Keycloak?
- What is OAuth2?
- What is JWT token?
- What is role-based access control (RBAC)?
- What roles did your application have?
- How did you integrate Keycloak with Spring Boot?
- Explain OAuth2 flow (Authorization Code Flow)
- What is the structure of JWT token?
- How do you validate JWT token in Spring Boot?
- What is the difference between authentication and authorization?
- How do you implement role-based access control?
- What is refresh token? Why is it needed?
- How do you secure REST endpoints?
- What is bearer token?

---

## Point 7: Monitoring & Observability

### Questions to Prepare:
- What is Prometheus?
- What is Grafana?
- What metrics did you monitor?
- What is Kibana?
- What is New Relic?
- How did you use these for troubleshooting?
- What metrics did you expose to Prometheus?
- How does Prometheus scrape metrics?
- What alerts did you configure in Grafana?
- How do you correlate logs with metrics?
- What is APM (Application Performance Monitoring)?
- How did you use Kibana for troubleshooting?
- How do you monitor API response times?
- What is observability?

---

## Point 8: Unit Testing

### Questions to Prepare:
- What is JUnit?
- What is Mockito?
- What is code coverage?
- Why do we use Mockito?
- Give example of a test you wrote
- What annotations did you use?
- What is the difference between unit test and integration test?
- Show me a sample test you wrote
- What is the difference between @Mock and @InjectMocks?
- How do you test exception scenarios?
- What is test coverage? Is 100% coverage necessary?
- What is TDD (Test-Driven Development)?
- How do you mock database calls?
- What is verify() in Mockito?

---

## Point 9-10: React.js Development

### Questions to Prepare:
- What is React.js?
- What is a React component?
- What is a custom hook?
- What dashboards did you build?
- How did you fetch data from backend?
- What is state management?
- What reusable components did you create?
- What is Virtual DOM? How does React use it?
- What is the difference between state and props?
- Explain useEffect hook with example
- What custom hooks did you create?
- How did you optimize React performance?
- How do you handle API calls in React?
- What is prop drilling? How did you avoid it?
- What is useState hook?
- What is the difference between class component and functional component?

---

## Point 11-13: Agile & Project Management

### Questions to Prepare:
- What is Agile?
- What is a sprint?
- What is JIRA?
- What is a user story?
- What is UAT?
- What ceremonies did you attend?
- How did you gather requirements?
- Explain Agile ceremonies you participated in
- How do you estimate story points?
- What is Definition of Done (DoD)?
- How do you handle changing requirements mid-sprint?
- What is velocity in Agile?
- How did you handle production bugs during sprint?
- What is sprint retrospective?
- What is backlog grooming?

---

## General Questions About Your Project (MobiLytix)

### Questions to Prepare:
- Explain your project in 2 minutes
- What does your application do?
- What was your role?
- What technologies did you use?
- How many team members?
- What was the biggest challenge in your project?
- What is the architecture of your application?
- How many microservices are there?
- What database schema did you design?
- How do you handle user authentication?
- What is the flow when user redeems an offer?
- How do you handle concurrent users?
- What was your biggest achievement in this project?

---

## Technology Fundamentals

### Questions to Prepare:
- What is Spring Boot?
- What is MySQL?
- Difference between MySQL and Redis?
- What is microservices architecture?
- What is REST API?
- What is dependency injection in Spring?
- What is Spring Boot starter?
- What is application.properties file?
- What is Maven/Gradle?
- What is Git?
- What are annotations in Spring Boot?
- What is @Autowired?
- What is @Service, @Repository, @Component?

---

## Scenario-Based Questions (Important!)

### Questions to Prepare:
- Your API is suddenly slow. How do you debug?
- User reports they're not seeing updated data. What could be wrong?
- Kafka consumer is not receiving messages. How do you troubleshoot?
- Database connection pool is exhausted. What do you do?
- Production deployment failed. How do you rollback?
- How do you handle if Redis is down?
- API is throwing 500 error. How do you investigate?
- Client says a feature is not working. What steps do you take?
- How do you handle if one microservice is down?
- Memory usage is very high. How do you debug?

---

## Code/Implementation Questions

### Questions to Prepare:
- Write a simple REST API controller
- Write a JUnit test case
- How do you read application.properties values?
- How do you call one microservice from another?
- Write code to fetch data from database using Spring Data JPA
- How do you publish message to Kafka?
- How do you consume message from Kafka?
- Write code to cache data in Redis
- How do you handle exceptions in Spring Boot?
- Write a simple React component

---

## Wipro Experience Questions

### Questions to Prepare:
- What did you do at Wipro?
- What testing did you perform?
- How did you reduce production errors by 30%?
- What tools did you use for testing?
- Why did you move from Wipro to Comviva?

---

## 🔥 Top 30 Critical Questions (MUST PREPARE)

1. Explain your project in 2 minutes
2. What microservices did you build?
3. How do microservices communicate?
4. What is a composite index? Give example
5. What did you cache in Redis? Why?
6. Explain Kafka producer and consumer
7. What is dead-letter queue?
8. Give examples of REST APIs you created
9. What HTTP status codes did you use?
10. What is JWT token?
11. How did you integrate Keycloak?
12. What is role-based access control?
13. What metrics did you monitor?
14. How do you troubleshoot using Kibana?
15. Show me a JUnit test you wrote
16. What is @Mock and @InjectMocks?
17. Explain useEffect hook
18. What custom hooks did you create?
19. How do you handle API calls in React?
20. What Agile ceremonies did you attend?
21. What is a sprint?
22. How do you estimate story points?
23. What is TTL in Redis?
24. What happens when cache expires?
25. How do you handle exceptions in Spring Boot?
26. What is the difference between PUT and PATCH?
27. How do you handle authentication in your APIs?
28. What is OAuth2 flow?
29. How do you optimize slow queries?
30. API is slow - how do you debug?

---

## 📝 Study Tips

- **Practice verbal explanations** for each question
- **Prepare specific examples** from your MobiLytix project
- **Draw diagrams** for architecture and flow questions
- **Code on paper** for implementation questions
- **Time yourself** - aim for 2-3 minute answers
- **Use the STAR method** for behavioral questions (Situation, Task, Action, Result)

---

**Good luck with your interview preparation! 🚀**

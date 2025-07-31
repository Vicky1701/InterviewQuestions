1. Can you briefly explain your role in the MobiLytix Loyalty Platform project?
I worked as a Java Full Stack Developer on the MobiLytix Loyalty Platform, an enterprise-grade digital rewards system. I contributed to both backend microservices (Spring Boot) and frontend (React.js), focusing on the Member Service initially and later leading the development of the Merchant Portal for business partners.

✅ 2. How is the microservices architecture designed in your project?
The platform has a modular microservices architecture with services like Member, Wallet, Catalogue, Fulfillment, and Notification. Each service owns its domain, communicates over REST APIs, and uses separate databases. Services are containerized with Docker and orchestrated through a CI/CD pipeline.

✅ 3. What is the purpose of the Member Service, and what features did you build there?
The Member Service manages the end-user lifecycle: registration, profile management, tier tracking. I implemented onboarding APIs, handled profile workflows, and ensured integration with the Wallet Service for real-time point updates. I also fixed critical data sync issues during the early release phases.

✅ 4. How did the Merchant Portal integrate with backend microservices?
The portal consumed REST APIs exposed by services like Catalogue, Campaign, and Wallet. I implemented frontend components using React and Ant Design, handled state management with React hooks, and ensured secure API communication using JWT-based authentication.

✅ 5. Why did your team choose a microservices architecture over monolithic?
We needed scalability, faster release cycles, and clear separation of business domains. Microservices allowed independent deployment, scaling of high-load services like Wallet, and flexibility for different development teams to work in parallel.

✅ 6. What technologies and tools did you use in the backend?
We used Spring Boot, Spring Data JPA, MySQL, Kafka for event handling, Redis for caching, and Keycloak for authentication. We also used JUnit, Mockito, and Postman for testing, and Jenkins for CI/CD.

✅ 7. How do the services communicate with each other in your system?
Primarily via REST APIs for synchronous calls, and Kafka for asynchronous messaging—for example, when a transaction triggers a notification or loyalty update. This ensures decoupling and better fault tolerance.

✅ 8. How did you handle authentication and authorization across services and the portal?
We used Keycloak for identity and access management. All services validated JWT tokens, and role-based access was enforced via API gateways and annotations in Spring Security. The frontend handled login and token management using Keycloak's JS adapter.

✅ 9. How do you ensure data consistency across services like Member, Wallet, and Fulfillment?
We used event-driven design with Kafka to maintain eventual consistency. For example, point redemption triggers a Kafka event consumed by both Wallet and Fulfillment services. We also had idempotency mechanisms to handle retries.

✅ 10. What challenges did you face while building or maintaining the Merchant Portal?
A major challenge was syncing real-time updates from the backend, especially in analytics dashboards. We implemented loading states, polling, and handled API errors gracefully. Performance optimization in rendering large tables was another challenge we solved using virtualization.

✅ 11. How did you implement real-time transaction processing in your system?
We used synchronous REST APIs for immediate feedback and Kafka for backend event processing. Wallet updates and reward redemptions happen in real-time and are validated instantly via API + DB operations, while Kafka ensures further async tasks like sending notifications.

✅ 12. How did you test your features? What types of testing were done?
I wrote unit tests with JUnit and Mockito for backend services, and integration tests to validate API interactions. On the frontend, we used Jest and React Testing Library for UI and component-level testing. We also had Postman test collections and test environments.

✅ 13. How did you handle error scenarios like redemption failures or service timeouts?
We used retry logic, proper HTTP status codes, circuit breakers for timeout handling, and custom error handlers for meaningful frontend messages. Logs were centralized using tools like ELK, and alerts were triggered for critical failures.

✅ 14. How did you manage the deployment and release process for your modules?
We used Jenkins for CI/CD pipelines. Code was built, tested, and deployed using Docker images. Each microservice had environment-specific YAMLs. Releases were managed via Git branching, and rollback scripts were in place for critical services.

✅ 15. How did the analytics/reporting module work in your system?
Transactional data was pushed to an analytics service via Kafka. We used batch jobs and real-time processing to aggregate metrics like redemption rates, campaign performance, and member engagement. This was visualized in the Merchant Portal.

✅ 16. How did you manage environment configurations (Dev/QA/Prod) for microservices?
We used Spring Profiles and externalized properties via config files or environment variables. Secrets were managed securely and deployment configs were version-controlled per environment.

✅ 17. How did you manage state in the React.js frontend of the Merchant Portal?
We primarily used React Hooks (useState, useEffect, useContext). For shared state like user session or catalog cache, we used a custom context provider. We avoided Redux to keep the architecture lean.

✅ 18. Can you describe a critical bug you resolved? What was your approach?
There was a major issue with inconsistent point balances due to delayed Kafka consumer processing. I traced the problem to out-of-order events and fixed it by introducing ordering guarantees and retry logic with exponential backoff. We added monitoring to avoid future occurrences.

✅ 19. How do you ensure security in your system?
All APIs were secured with JWT tokens issued by Keycloak. Role-based access was enforced using annotations and Keycloak client roles. Frontend used HTTPS, token expiry handling, and CORS policies to prevent misuse.

✅ 20. What performance or scalability challenges did your platform face and how did you solve them?
The Wallet Service initially struggled under high load. We solved it by introducing caching with Redis, optimizing DB indexes, and using bulk processing for high-volume events. Services were scaled horizontally using containerized deployment strategies.

# Unit Testing with JUnit & Mockito

## 1. ✅ What is Unit Testing & Why is it Important
- **Definition of unit test**: Testing individual components (units) of code in isolation
- **Benefits**:
  - Catch bugs early in development
  - Improve code quality and design
  - Enable safe refactoring
  - Serve as living documentation

## 2. ✅ JUnit Basics
- **What is JUnit?**: Most popular Java unit testing framework
- **Popular versions**:
  - JUnit 4 (legacy)
  - JUnit 5 (Jupiter, current standard)
- **Maven/Gradle dependencies**:
  ```xml
  <!-- JUnit 5 -->
  <dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.2</version>
    <scope>test</scope>
  </dependency>

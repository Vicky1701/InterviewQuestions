# Spring Security - Interview Questions & Answers

## 🎯 **Essential Questions for 2+ Years Experience**

---

## **1. What is Spring Security and what are its core features?**

**Answer:**
Spring Security is a comprehensive security framework for Java applications that provides authentication, authorization, and protection against common security vulnerabilities.

**Core Features:**

1. **Authentication** - Verifying user identity
2. **Authorization** - Controlling access to resources
3. **CSRF Protection** - Cross-Site Request Forgery protection
4. **Session Management** - Session fixation protection, concurrent session control
5. **Password Encryption** - BCrypt, SCrypt, Argon2 support
6. **OAuth2/JWT** - Modern authentication protocols
7. **Method-level Security** - Annotation-based security
8. **Web Security** - URL-based access control

**Basic Configuration:**
```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/**").hasAnyRole("USER", "ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutUrl("/logout")
                .logoutSuccessUrl("/login?logout")
                .invalidateHttpSession(true)
            )
            .csrf(csrf -> csrf.disable())
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED)
                .maximumSessions(1)
                .maxSessionsPreventsLogin(false)
            );
        
        return http.build();
    }
}
```

---

## **2. What is the difference between Authentication and Authorization?**

**Answer:**

| Aspect | Authentication | Authorization |
|--------|---------------|---------------|
| **Purpose** | Verify identity | Control access |
| **Question** | Who are you? | What can you do? |
| **Process** | Login with credentials | Check permissions |
| **When** | First step | After authentication |
| **Example** | Username/password | Role-based access |

**Authentication Example:**
```java
@Service
public class CustomAuthenticationService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    public Authentication authenticate(String username, String password) {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new BadCredentialsException("Invalid credentials"));
        
        if (!passwordEncoder.matches(password, user.getPassword())) {
            throw new BadCredentialsException("Invalid credentials");
        }
        
        // Authentication successful
        Collection<GrantedAuthority> authorities = user.getRoles().stream()
            .map(role -> new SimpleGrantedAuthority("ROLE_" + role.getName()))
            .collect(Collectors.toList());
        
        return new UsernamePasswordAuthenticationToken(username, null, authorities);
    }
}
```

**Authorization Example:**
```java
@RestController
@RequestMapping("/api")
public class UserController {
    
    // Method-level authorization
    @PreAuthorize("hasRole('ADMIN')")
    @GetMapping("/admin/users")
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @PreAuthorize("hasRole('USER') or hasRole('ADMIN')")
    @GetMapping("/profile")
    public User getProfile(Authentication authentication) {
        return userService.findByUsername(authentication.getName());
    }
    
    @PreAuthorize("@userService.isOwner(authentication.name, #userId) or hasRole('ADMIN')")
    @PutMapping("/users/{userId}")
    public User updateUser(@PathVariable Long userId, @RequestBody UserRequest request) {
        return userService.updateUser(userId, request);
    }
}
```

---

## **3. How do you implement JWT authentication in Spring Boot?**

**Answer:**
JWT (JSON Web Token) provides stateless authentication for REST APIs.

**1. Dependencies:**
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
</dependency>
```

**2. JWT Utility Class:**
```java
@Component
public class JwtTokenUtil {
    
    private String jwtSecret = "mySecretKey";
    private int jwtExpirationInMs = 604800000; // 7 days
    
    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        Collection<? extends GrantedAuthority> authorities = userDetails.getAuthorities();
        claims.put("roles", authorities.stream()
            .map(GrantedAuthority::getAuthority)
            .collect(Collectors.toList()));
        return createToken(claims, userDetails.getUsername());
    }
    
    private String createToken(Map<String, Object> claims, String subject) {
        return Jwts.builder()
            .setClaims(claims)
            .setSubject(subject)
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + jwtExpirationInMs))
            .signWith(SignatureAlgorithm.HS512, jwtSecret)
            .compact();
    }
    
    public Boolean validateToken(String token, UserDetails userDetails) {
        final String username = getUsernameFromToken(token);
        return (username.equals(userDetails.getUsername()) && !isTokenExpired(token));
    }
    
    public String getUsernameFromToken(String token) {
        return getClaimFromToken(token, Claims::getSubject);
    }
    
    public Date getExpirationDateFromToken(String token) {
        return getClaimFromToken(token, Claims::getExpiration);
    }
    
    public <T> T getClaimFromToken(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = getAllClaimsFromToken(token);
        return claimsResolver.apply(claims);
    }
    
    private Claims getAllClaimsFromToken(String token) {
        return Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(token).getBody();
    }
    
    private Boolean isTokenExpired(String token) {
        final Date expiration = getExpirationDateFromToken(token);
        return expiration.before(new Date());
    }
}
```

**3. JWT Authentication Filter:**
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    
    @Autowired
    private UserDetailsService userDetailsService;
    
    @Autowired
    private JwtTokenUtil jwtTokenUtil;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                  HttpServletResponse response, 
                                  FilterChain filterChain) throws ServletException, IOException {
        
        final String requestTokenHeader = request.getHeader("Authorization");
        
        String username = null;
        String jwtToken = null;
        
        if (requestTokenHeader != null && requestTokenHeader.startsWith("Bearer ")) {
            jwtToken = requestTokenHeader.substring(7);
            try {
                username = jwtTokenUtil.getUsernameFromToken(jwtToken);
            } catch (IllegalArgumentException e) {
                logger.error("Unable to get JWT Token");
            } catch (ExpiredJwtException e) {
                logger.error("JWT Token has expired");
            }
        }
        
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);
            
            if (jwtTokenUtil.validateToken(jwtToken, userDetails)) {
                UsernamePasswordAuthenticationToken authToken = 
                    new UsernamePasswordAuthenticationToken(
                        userDetails, null, userDetails.getAuthorities());
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        
        filterChain.doFilter(request, response);
    }
}
```

**4. Security Configuration:**
```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class JwtSecurityConfig {
    
    @Autowired
    private JwtAuthenticationEntryPoint jwtAuthenticationEntryPoint;
    
    @Autowired
    private JwtAuthenticationFilter jwtAuthenticationFilter;
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration authConfig) throws Exception {
        return authConfig.getAuthenticationManager();
    }
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers("/api/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .exceptionHandling(ex -> ex
                .authenticationEntryPoint(jwtAuthenticationEntryPoint)
            )
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            );
        
        http.addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);
        
        return http.build();
    }
}
```

**5. Authentication Controller:**
```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {
    
    @Autowired
    private AuthenticationManager authenticationManager;
    
    @Autowired
    private JwtTokenUtil jwtTokenUtil;
    
    @Autowired
    private UserDetailsService userDetailsService;
    
    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginRequest loginRequest) {
        try {
            Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(
                    loginRequest.getUsername(), 
                    loginRequest.getPassword())
            );
            
            UserDetails userDetails = userDetailsService.loadUserByUsername(loginRequest.getUsername());
            String token = jwtTokenUtil.generateToken(userDetails);
            
            return ResponseEntity.ok(new JwtResponse(token, userDetails.getUsername()));
            
        } catch (BadCredentialsException e) {
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED)
                .body(new ErrorResponse("Invalid credentials"));
        }
    }
    
    @PostMapping("/refresh")
    public ResponseEntity<?> refreshToken(@RequestBody TokenRefreshRequest request) {
        String refreshToken = request.getRefreshToken();
        
        if (jwtTokenUtil.validateRefreshToken(refreshToken)) {
            String username = jwtTokenUtil.getUsernameFromToken(refreshToken);
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);
            String newToken = jwtTokenUtil.generateToken(userDetails);
            
            return ResponseEntity.ok(new JwtResponse(newToken, username));
        }
        
        return ResponseEntity.status(HttpStatus.FORBIDDEN)
            .body(new ErrorResponse("Invalid refresh token"));
    }
}
```

---

## **4. What are the different ways to configure Spring Security?**

**Answer:**
Spring Security can be configured in multiple ways:

### **1. Java Configuration (Recommended):**
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
            )
            .httpBasic(Customizer.withDefaults());
        
        return http.build();
    }
    
    @Bean
    public UserDetailsService userDetailsService() {
        return new CustomUserDetailsService();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

### **2. Method-level Security:**
```java
@Configuration
@EnableGlobalMethodSecurity(
    prePostEnabled = true,     // @PreAuthorize, @PostAuthorize
    securedEnabled = true,     // @Secured
    jsr250Enabled = true       // @RolesAllowed
)
public class MethodSecurityConfig {
    
    @Bean
    public MethodSecurityExpressionHandler methodSecurityExpressionHandler() {
        DefaultMethodSecurityExpressionHandler expressionHandler = 
            new DefaultMethodSecurityExpressionHandler();
        expressionHandler.setPermissionEvaluator(new CustomPermissionEvaluator());
        return expressionHandler;
    }
}

@Service
public class UserService {
    
    @PreAuthorize("hasRole('ADMIN')")
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    @PreAuthorize("#username == authentication.name or hasRole('ADMIN')")
    public User getUserByUsername(@Param("username") String username) {
        return userRepository.findByUsername(username);
    }
    
    @PostAuthorize("returnObject.username == authentication.name or hasRole('ADMIN')")
    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
    
    @Secured({"ROLE_ADMIN", "ROLE_MANAGER"})
    public void deleteUser(Long userId) {
        userRepository.deleteById(userId);
    }
    
    @RolesAllowed("ADMIN")
    public void updateUserRoles(Long userId, Set<String> roles) {
        // Update user roles
    }
}
```

### **3. Multiple Security Configurations:**
```java
@Configuration
@EnableWebSecurity
public class MultipleSecurityConfig {
    
    @Configuration
    @Order(1)
    public static class ApiWebSecurityConfigurationAdapter {
        
        @Bean
        public SecurityFilterChain apiFilterChain(HttpSecurity http) throws Exception {
            http
                .securityMatcher("/api/**")
                .authorizeHttpRequests(authz -> authz
                    .anyRequest().hasRole("API_USER")
                )
                .httpBasic(Customizer.withDefaults())
                .sessionManagement(session -> session
                    .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                );
            return http.build();
        }
    }
    
    @Configuration
    @Order(2)
    public static class FormLoginWebSecurityConfigurationAdapter {
        
        @Bean
        public SecurityFilterChain formLoginFilterChain(HttpSecurity http) throws Exception {
            http
                .authorizeHttpRequests(authz -> authz
                    .requestMatchers("/admin/**").hasRole("ADMIN")
                    .anyRequest().authenticated()
                )
                .formLogin(form -> form
                    .loginPage("/login")
                    .permitAll()
                );
            return http.build();
        }
    }
}
```

### **4. Custom Authentication Provider:**
```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {
    
    @Autowired
    private UserService userService;
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        
        String username = authentication.getName();
        String password = authentication.getCredentials().toString();
        
        User user = userService.findByUsername(username);
        if (user == null) {
            throw new BadCredentialsException("Invalid credentials");
        }
        
        if (!passwordEncoder.matches(password, user.getPassword())) {
            throw new BadCredentialsException("Invalid credentials");
        }
        
        if (!user.isEnabled()) {
            throw new DisabledException("Account is disabled");
        }
        
        Collection<GrantedAuthority> authorities = user.getRoles().stream()
            .map(role -> new SimpleGrantedAuthority("ROLE_" + role.getName()))
            .collect(Collectors.toList());
        
        return new UsernamePasswordAuthenticationToken(username, password, authorities);
    }
    
    @Override
    public boolean supports(Class<?> authentication) {
        return authentication.equals(UsernamePasswordAuthenticationToken.class);
    }
}

@Configuration
public class AuthConfig {
    
    @Autowired
    private CustomAuthenticationProvider customAuthenticationProvider;
    
    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration authConfig) throws Exception {
        
        AuthenticationManagerBuilder builder = 
            new AuthenticationManagerBuilder((ObjectPostProcessor) authConfig);
        builder.authenticationProvider(customAuthenticationProvider);
        
        return builder.build();
    }
}
```

---

## **5. How do you handle password encoding in Spring Security?**

**Answer:**
Spring Security provides various password encoders to securely store passwords.

### **Available Password Encoders:**

| Encoder | Use Case | Strength |
|---------|----------|----------|
| **BCryptPasswordEncoder** | Most common, adaptive | High |
| **SCryptPasswordEncoder** | Memory-hard function | Very High |
| **Argon2PasswordEncoder** | Latest, most secure | Highest |
| **Pbkdf2PasswordEncoder** | PBKDF2 with HMAC-SHA | High |

### **BCrypt Implementation:**
```java
@Configuration
public class PasswordConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(12); // Strength 12
    }
}

@Service
public class UserService {
    
    @Autowired
    private PasswordEncoder passwordEncoder;
    
    @Autowired
    private UserRepository userRepository;
    
    public User createUser(UserRegistrationRequest request) {
        // Encode password before saving
        String encodedPassword = passwordEncoder.encode(request.getPassword());
        
        User user = User.builder()
            .username(request.getUsername())
            .email(request.getEmail())
            .password(encodedPassword)
            .enabled(true)
            .build();
        
        return userRepository.save(user);
    }
    
    public boolean validatePassword(String rawPassword, String encodedPassword) {
        return passwordEncoder.matches(rawPassword, encodedPassword);
    }
    
    public User changePassword(String username, String oldPassword, String newPassword) {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UserNotFoundException("User not found"));
        
        // Verify old password
        if (!passwordEncoder.matches(oldPassword, user.getPassword())) {
            throw new BadCredentialsException("Invalid old password");
        }
        
        // Encode and set new password
        String encodedNewPassword = passwordEncoder.encode(newPassword);
        user.setPassword(encodedNewPassword);
        
        return userRepository.save(user);
    }
}
```

### **Multiple Encoders (DelegatingPasswordEncoder):**
```java
@Configuration
public class PasswordConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        String idForEncode = "bcrypt";
        Map<String, PasswordEncoder> encoders = new HashMap<>();
        
        encoders.put(idForEncode, new BCryptPasswordEncoder());
        encoders.put("noop", NoOpPasswordEncoder.getInstance());
        encoders.put("pbkdf2", new Pbkdf2PasswordEncoder());
        encoders.put("scrypt", new SCryptPasswordEncoder());
        encoders.put("argon2", new Argon2PasswordEncoder());
        
        return new DelegatingPasswordEncoder(idForEncode, encoders);
    }
}
```

### **Password Policies:**
```java
@Component
public class PasswordValidator {
    
    private static final String PASSWORD_PATTERN = 
        "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*[@#$%^&+=])(?=\\S+$).{8,}$";
    
    private static final Pattern pattern = Pattern.compile(PASSWORD_PATTERN);
    
    public boolean isValid(String password) {
        return pattern.matcher(password).matches();
    }
    
    public void validatePassword(String password) {
        if (password == null || password.length() < 8) {
            throw new WeakPasswordException("Password must be at least 8 characters long");
        }
        
        if (!password.matches(".*[A-Z].*")) {
            throw new WeakPasswordException("Password must contain at least one uppercase letter");
        }
        
        if (!password.matches(".*[a-z].*")) {
            throw new WeakPasswordException("Password must contain at least one lowercase letter");
        }
        
        if (!password.matches(".*\\d.*")) {
            throw new WeakPasswordException("Password must contain at least one digit");
        }
        
        if (!password.matches(".*[@#$%^&+=].*")) {
            throw new WeakPasswordException("Password must contain at least one special character");
        }
    }
}
```

---

## **6. How do you implement Role-based and Permission-based authorization?**

**Answer:**

### **Role-based Authorization:**
```java
// Entity classes
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String username;
    private String password;
    private boolean enabled;
    
    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
}

@Entity
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Enumerated(EnumType.STRING)
    private RoleName name;
    
    @ManyToMany(mappedBy = "roles")
    private Set<User> users = new HashSet<>();
}

public enum RoleName {
    ROLE_USER,
    ROLE_ADMIN,
    ROLE_MANAGER
}

// UserDetailsService implementation
@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    @Transactional
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));
        
        return UserPrincipal.create(user);
    }
}

public class UserPrincipal implements UserDetails {
    private Long id;
    private String username;
    private String password;
    private Collection<? extends GrantedAuthority> authorities;
    
    public static UserPrincipal create(User user) {
        List<GrantedAuthority> authorities = user.getRoles().stream()
            .map(role -> new SimpleGrantedAuthority(role.getName().name()))
            .collect(Collectors.toList());
        
        return new UserPrincipal(
            user.getId(),
            user.getUsername(),
            user.getPassword(),
            authorities
        );
    }
    
    // Implement UserDetails methods...
}
```

### **Permission-based Authorization:**
```java
// Extended entities for permissions
@Entity
public class Permission {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name; // READ_USER, WRITE_USER, DELETE_USER
    private String resource; // USER, ORDER, PRODUCT
    
    @ManyToMany(mappedBy = "permissions")
    private Set<Role> roles = new HashSet<>();
}

@Entity
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
        name = "role_permissions",
        joinColumns = @JoinColumn(name = "role_id"),
        inverseJoinColumns = @JoinColumn(name = "permission_id")
    )
    private Set<Permission> permissions = new HashSet<>();
    
    @ManyToMany(mappedBy = "roles")
    private Set<User> users = new HashSet<>();
}

// Custom Permission Evaluator
@Component
public class CustomPermissionEvaluator implements PermissionEvaluator {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    public boolean hasPermission(Authentication authentication, Object targetDomainObject, Object permission) {
        if (authentication == null || targetDomainObject == null || !(permission instanceof String)) {
            return false;
        }
        
        String targetType = targetDomainObject.getClass().getSimpleName().toUpperCase();
        return hasPrivilege(authentication, targetType, permission.toString().toUpperCase());
    }
    
    @Override
    public boolean hasPermission(Authentication authentication, Serializable targetId, String targetType, Object permission) {
        if (authentication == null || targetType == null || !(permission instanceof String)) {
            return false;
        }
        
        return hasPrivilege(authentication, targetType.toUpperCase(), permission.toString().toUpperCase());
    }
    
    private boolean hasPrivilege(Authentication authentication, String targetType, String permission) {
        User user = userRepository.findByUsername(authentication.getName()).orElse(null);
        if (user == null) {
            return false;
        }
        
        return user.getRoles().stream()
            .flatMap(role -> role.getPermissions().stream())
            .anyMatch(perm -> perm.getResource().equals(targetType) && perm.getName().equals(permission));
    }
}

// Usage in controllers and services
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PreAuthorize("hasRole('ADMIN')")
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @PreAuthorize("hasPermission(#userId, 'USER', 'READ')")
    @GetMapping("/{userId}")
    public User getUser(@PathVariable Long userId) {
        return userService.findById(userId);
    }
    
    @PreAuthorize("hasPermission(#user, 'WRITE')")
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
    
    @PreAuthorize("hasPermission(#userId, 'USER', 'DELETE') or @userService.isOwner(authentication.name, #userId)")
    @DeleteMapping("/{userId}")
    public void deleteUser(@PathVariable Long userId) {
        userService.deleteById(userId);
    }
}
```

---

## **🎯 Quick Review Questions**

1. **What is CSRF and how does Spring Security protect against it?**
   - Cross-Site Request Forgery protection using CSRF tokens

2. **How do you handle CORS in Spring Security?**
   - Configure CORS using `@CrossOrigin` or global CORS configuration

3. **What is the difference between @Secured and @PreAuthorize?**
   - `@Secured` supports only role-based, `@PreAuthorize` supports SpEL expressions

4. **How do you implement Remember Me functionality?**
   - Configure remember-me with token repository

5. **What is Session Fixation and how to prevent it?**
   - Create new session after authentication to prevent session hijacking

---

**🚀 Pro Tips for Interview Success:**
- Understand the security filter chain
- Know different authentication mechanisms
- Practice JWT implementation
- Understand role vs permission-based authorization
- Be familiar with common security vulnerabilities

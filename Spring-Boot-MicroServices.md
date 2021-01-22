# SPRING BOOT MICRO SERVICES

##### Course Enrollment Microservices, Spring Cloud, Spring Boot, Angular 8, MySQL, Hibernate, Liquibase

The application structure is as follows.
- **microservice-user-management** - Microservice implemented using Spring boot. [More info](microservice-user-management/README.md)
- **microservice-course-management** - Microservice implemented using Spring boot. [More info](microservice-course-management/README.md)
- **eureka-discovery-service** - Microservice implemented using Spring eureka. [More info](eureka-discovery-service/README.md)
- **zuul-gateway-service** - Microservice implemented using Spring zuul. [More info](zuul-gateway-service/README.md)
- **client-side** - A NodeJs application implemented using Angular 8. This consumes services hosted by server side.  [More info](client-side/README.md)

##### Build
1) Build Spring Boot microservices
```
$ cd microservice path
$ gradlew bootJar
$ gradlew bootRun
```
2) Build and run client side application
```
$ cd client-side
$ npm install
$ ng serve
```

Access application using following URL
```
http://localhost:4200
```
REF : 
```
https://github.com/XMENCODE/angular8-springboot-microservices
```


##### EUREKA DISCOVERY SERVER
Create a spring boot application to reister and enable EurekaDiscoveryService


@SpringBootApplication
@EnableEurekaServer
public class EurekaDiscoveryServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(EurekaDiscoveryServiceApplication.class, args);
	}
    }


##### application.properties
```
server.port=8761

#telling the server not to register himself in the service

eureka.client.register-with-eureka=false

#Eureka clients fetch the service registry (ServiceInstance: {URL, PORT, HOST}) from the Eureka server

eureka.client.fetch-registry=true
```

##### Micro Services
1. 
2.

Create individual Spring Boot Apps for these two Micro services

##### MicroServiceCourseManagementApplication

Create a spring boot app for MicroserviceCourseManagementApplication

@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients
public class MicroserviceCourseManagementApplication {

	public static void main(String[] args) {
		SpringApplication.run(MicroserviceCourseManagementApplication.class, args);
	}

	@Bean
	public WebMvcConfigurer corsConfigurer() {
		return new WebMvcConfigurer() {
			@Override
			public void addCorsMappings(CorsRegistry registry) {
				registry.addMapping("/**").allowedOrigins("*");
			}
		};
	}
    }

```
folder : resources
         application.properties
         db : subFolder
                 changelog : subFolder
                            db.changelog-1.0
                            db.changelog-master

```

##### application.properties

```
spring.application.name=course-service
server.port=8001

#datasource
spring.datasource.url=jdbc:mysql://localhost:3306/micro_course?useUnicode=true&useLegacyDatetimeCode=false&serverTimezone=UTC&createDatabaseIfNotExist=true&allowPublicKeyRetrieval=true&useSSL=true
#you should change them according to your credentials.
spring.datasource.username=root
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

#disable hibernate auto ddl changes
spring.jpa.hibernate.ddl-auto=none

#liquibase
spring.liquibase.change-log=classpath:/db/changelog/db.changelog-master.xml


#eureka
eureka.client.service-url.default-zone=http://localhost:8761/eureka/
#indicates the frequency the client sends heartbeats to server to indicate that it is alive.
eureka.instance.lease-renewal-interval-in-seconds=30
#indicates the duration the server waits since it received the last heartbeat before it can evict an instance from its registry.
eureka.instance.lease-expiration-duration-in-seconds=90

#load balancing
ribbon.eureka.enabled=true
```

##### changelog

* db.changelog-1.0

```
<?xml version="1.0" encoding="UTF-8"?>

<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.0.xsd
        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">

    <changeSet id="1" author="sha">
        <sql>
            CREATE TABLE course (
            id BIGINT NOT NULL AUTO_INCREMENT,
            title VARCHAR(255) NOT NULL,
            author VARCHAR(255) NOT NULL,
            category VARCHAR(255),
            publish_date DATE,
            CONSTRAINT pk_id PRIMARY KEY (id)
            );
        </sql>
        <rollback>
            DROP TABLE course;
        </rollback>
    </changeSet>
    <changeSet id="2" author="sha">
        <sql>
            CREATE TABLE transaction (
            id BIGINT NOT NULL AUTO_INCREMENT,
            course_id BIGINT NOT NULL,
            user_id BIGINT NOT NULL,
            date_of_issue DATETIME,
            CONSTRAINT pk_id PRIMARY KEY (id),
            CONSTRAINT fk_tran_course FOREIGN KEY (course_id) REFERENCES course(id) ON DELETE CASCADE ON UPDATE CASCADE
            );
        </sql>
        <rollback>
            DROP TABLE transaction;
        </rollback>
    </changeSet>
    <changeSet id="3" author="sha">
        <sql>
            INSERT INTO course (title, author, category, publish_date) VALUES('Microservices', 'Instructor 1', 'Programming', NOW());
            INSERT INTO course (title, author, category, publish_date) VALUES('Java Programming', 'Instructor 2', 'Programming', NOW());
            INSERT INTO course (title, author, category, publish_date) VALUES('Web Development', 'Instructor 3', 'Web', NOW());
            INSERT INTO course (title, author, category, publish_date) VALUES('Mobile Application', 'Instructor 4', 'Mobile', NOW());
            INSERT INTO course (title, author, category, publish_date) VALUES('Amazon Web Services', 'Instructor 5', 'Administration', NOW());
        </sql>
        <rollback>
            TRUNCATE TABLE course;
        </rollback>
    </changeSet>
</databaseChangeLog>

```

* db.changelog-master

```
<?xml version="1.0" encoding="UTF-8"?>

<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.0.xsd
        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">
    <include file="db/changelog/db.changelog-1.0.xml"/>
</databaseChangeLog>
```



* controller

@RestController
public class CourseController {

    @Autowired
    private UserClient userClient;

    @Autowired
    private CourseService courseService;

    @Autowired
    private DiscoveryClient discoveryClient;

    @Autowired
    private Environment env;

    @Value("${spring.application.name}")
    private String serviceId;

    @GetMapping("/service/port")
    public String getPort(){
        return "Service is working at port : " + env.getProperty("local.server.port");
    }

    @GetMapping("/service/instances")
    public ResponseEntity<?> getInstances() {
        return ResponseEntity.ok(discoveryClient.getInstances(serviceId));
    }

    @GetMapping("/service/user/{userId}")
    public ResponseEntity<?> findTransactionsOfUser(@PathVariable Long userId){
        return ResponseEntity.ok(courseService.findTransactionsOfUser(userId));
    }

    @GetMapping("/service/all")
    public ResponseEntity<?> findAllCourses(){
        return ResponseEntity.ok(courseService.allCourses());
    }

    @PostMapping("/service/enroll")
    public ResponseEntity<?> saveTransaction(@RequestBody Transaction transaction) {
        transaction.setDateOfIssue(LocalDateTime.now());
        transaction.setCourse(courseService.findCourseById(transaction.getCourse().getId()));
        return new ResponseEntity<>(courseService.saveTransaction(transaction), HttpStatus.CREATED);
    }

    @GetMapping("/service/course/{courseId}")
    public ResponseEntity<?> findStudentsOfCourse(@PathVariable Long courseId){
        List<Transaction> transactions = courseService.findTransactionsOfCourse(courseId);
        if(CollectionUtils.isEmpty(transactions)){
           return ResponseEntity.notFound().build();
        }
        List<Long> userIdList = transactions.parallelStream().map(t -> t.getUserId()).collect(Collectors.toList());
        List<String> students = userClient.getUserNames(userIdList);
        return ResponseEntity.ok(students);
    }
    }

* interComm

@FeignClient("user-service")
public interface UserClient {

    @RequestMapping(method = RequestMethod.POST, value = "/service/names", consumes = "application/json")
    List<String> getUserNames(@RequestBody List<Long> userIdList);
    }

* model

@Data
@Entity
@Table(name = "course")
public class Course implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "title")
    private String title;

    @Column(name = "author")
    private String author;

    @Column(name = "category")
    private String category;

    @Column(name = "publish_date")
    private LocalDate publishDate;
    }


@Data
@Entity
@Table(name = "transaction")
public class Transaction implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "course_id", referencedColumnName = "id")
    private Course course;

    @Column(name = "user_id")
    private Long userId;

    @Column(name = "date_of_issue")
    private LocalDateTime dateOfIssue;
    }

* repository

public interface CourseRepository extends JpaRepository<Course, Long> {
}

public interface TransactionRepository extends JpaRepository<Transaction, Long> {

    List<Transaction> findAllByUserId(Long userId);

    List<Transaction> findAllByCourseId(Long courseId);
}


* service

public interface CourseService {
    List<Course> allCourses();

    Course findCourseById(Long courseId);

    List<Transaction> findTransactionsOfUser(Long userId);

    List<Transaction> findTransactionsOfCourse(Long courseId);

    Transaction saveTransaction(Transaction transaction);
    }

@Service
public class CourseServiceImpl implements CourseService{

    @Autowired
    private CourseRepository courseRepository;

    @Autowired
    private TransactionRepository transactionRepository;

    @Override
    public List<Course> allCourses() {
        return courseRepository.findAll();
    }

    @Override
    public Course findCourseById(Long courseId) {
        return courseRepository.findById(courseId).orElse(null);
    }

    @Override
    public List<Transaction> findTransactionsOfUser(Long userId) {
        return transactionRepository.findAllByUserId(userId);
    }

    @Override
    public List<Transaction> findTransactionsOfCourse(Long courseId) {
        return transactionRepository.findAllByCourseId(courseId);
    }

    @Override
    public Transaction saveTransaction(Transaction transaction) {
        return transactionRepository.save(transaction);
    }
    }

##### MicroserviceUserManagementApplication

@SpringBootApplication
@EnableDiscoveryClient
public class MicroserviceUserManagementApplication {

	public static void main(String[] args) {
		SpringApplication.run(MicroserviceUserManagementApplication.class, args);
	}
    }


application.properties
```
server.port=8000
spring.application.name=user-service
spring.datasource.url=jdbc:mysql://localhost:3306/micro_user?useUnicode=true&useLegacyDatetimeCode=false&serverTimezone=UTC&createDatabaseIfNotExist=true&allowPublicKeyRetrieval=true&useSSL=false
spring.datasource.username=root
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

#none,create,update,validate
spring.jpa.hibernate.ddl-auto=none

#liquibase
spring.liquibase.change-log=classpath:/db/changelog/db.changelog-master.xml

#eureka
eureka.client.service-url.default-zone=http://localhost:8761/eureka/
#indicates the frequency the client sends heartbeat to server to indicate that it is alive.
eureka.instance.lease-renewal-interval-in-seconds=30
#indicates the duration the server waits since it received the last heartbeat before it can evict an instance from its registry
eureka.instance.lease-expiration-duration-in-seconds=90

#load balancing
ribbon.eureka.enabled=true
```

* db.changelog-1.0

```
<?xml version="1.0" encoding="UTF-8"?>

<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.0.xsd
        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">

    <changeSet id="1" author="senol">
        <sql>
            CREATE TABLE user (
                id BIGINT NOT NULL AUTO_INCREMENT,
                username VARCHAR(255) NOT NULL,
                password VARCHAR(255) NOT NULL,
                name VARCHAR(255) NOT NULL,
                role VARCHAR(10) NOT NULL,
                CONSTRAINT pk_id PRIMARY KEY (id)
            );
        </sql>
        <rollback>
            DROP TABLE user;
        </rollback>
    </changeSet>

    <changeSet id="2" author="senol">
        <sql>
            ALTER TABLE user ADD email varchar(255);
        </sql>
        <rollback>
            ALTER TABLE user DROP COLUMN email;
        </rollback>
    </changeSet>
</databaseChangeLog>
```

* db.changelog-master

```
<?xml version="1.0" encoding="UTF-8"?>

<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.0.xsd
        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">

    <include file="db/changelog/db.changelog-1.0.xml"/>
</databaseChangeLog>
```

* config

@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private UserDetailsService userDetailsService;

    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //Cross origin resource sharing
        http.cors().and()
                //starts authorizing configurations.
                .authorizeRequests()
                //ignoring the guest's urls...
                .antMatchers("/resources/**", "/error", "/service/**").permitAll()
                //authenticate all remaining URLs.
                .anyRequest().fullyAuthenticated()
                .and()
                .logout().permitAll()
                .logoutRequestMatcher(new AntPathRequestMatcher("/service/logout", "POST"))
                //login form
                .and()
                .formLogin().loginPage("/service/login").and()
                //enable basic header authentication.
                .httpBasic().and()
                //cross-side request forgery.
                .csrf().disable();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
    }

    @Bean
    public WebMvcConfigurer corsConfigurer(){
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
            registry.addMapping("/**").allowedOrigins("*");
            }
        };
    }
    }

* controller

@RestController
public class UserController {

    @Autowired
    private UserService userService;

    @Autowired
    private DiscoveryClient discoveryClient;

    @Autowired
    private Environment env;

    @Value("${spring.application.name}")
    private String serviceId;

    @GetMapping("/service/port")
    public String getPort(){
        return "Service port number : " + env.getProperty("local.server.port");
    }

    @GetMapping("/service/instances")
    public ResponseEntity<?> getInstances(){
        return new ResponseEntity<>(discoveryClient.getInstances(serviceId), HttpStatus.OK);
    }

    @GetMapping("/service/services")
    public ResponseEntity<?> getServices(){
        return new ResponseEntity<>(discoveryClient.getServices(), HttpStatus.OK);
    }

    @PostMapping("/service/registration")
    public ResponseEntity<?> saveUser(@RequestBody User user){
        if(userService.findByUsername(user.getUsername()) != null){
            //Status Code: 409
            return new ResponseEntity<>(HttpStatus.CONFLICT);
        }
        //Default role = user
        user.setRole(Role.USER);
        return new ResponseEntity<>(userService.save(user), HttpStatus.CREATED);
    }

    @GetMapping("/service/login")
    public ResponseEntity<?> getUser(Principal principal){
        //Principal principal = request.getUserPrincipal();
        if(principal == null || principal.getName() == null){
            //This means; logout will be successful. login?logout
            return new ResponseEntity<>(HttpStatus.OK);
        }
        //username = principal.getName()
        return ResponseEntity.ok(userService.findByUsername(principal.getName()));
    }

    @PostMapping("/service/names")
    public ResponseEntity<?> getNamesOfUsers(@RequestBody List<Long> idList){
        return ResponseEntity.ok(userService.findUsers(idList));
    }

    @GetMapping("/service/test")
    public ResponseEntity<?> test(){
        return ResponseEntity.ok("It is working...");
    }
    }

* model

public enum Role {
    USER,
    ADMIN
}

@Data
@Entity
@Table(name = "user")
public class User {

    @Id
    //Generation Types:
    //Auto: Default one. It does not take any specific action.
    //Identity: Auto increment.
    //Sequence: Oracle or Posgresql creates variable to auto increment.
    //Table: Hibernate uses a database table to simulate a sequence.
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "name")
    private String name;

    @Column(name = "username")
    private String username;

    @Column(name = "password")
    private String password;

    @Enumerated(value = EnumType.STRING)
    @Column(name = "role")
    private Role role;
    }

* repository

public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByUsername(String username);

    @Query("select u.name from User u where u.id in (:pIdList)")
    List<String> findByIdList(@Param("pIdList") List<Long> idList);
    }


* service

public interface UserService {
    User save(User user);

    User findByUsername(String username);

    List<String> findUsers(List<Long> idList);
    }


@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserRepository userRepository;

    //We will create bean for it in security config.
    @Autowired
    private PasswordEncoder passwordEncoder;

    @Override
    public User save(User user){
        user.setPassword(passwordEncoder.encode(user.getPassword()));
        return userRepository.save(user);
    }

    @Override
    public User findByUsername(String username){
        return userRepository.findByUsername(username).orElse(null);
    }

    @Override
    public List<String> findUsers(List<Long> idList){
        return userRepository.findByIdList(idList);
    }
    }



@Service
public class UserDetailServiceImpl implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username).orElse(null);
        if(user == null){
            throw new UsernameNotFoundException(username);
        }
        Set<GrantedAuthority> authorities = new HashSet<>();
        authorities.add(new SimpleGrantedAuthority(user.getRole().name()));

        return new org.springframework.security.core.userdetails.User(username, user.getPassword(), authorities);
    }
    }


##### ZuulGatewayServiceApplication

@SpringBootApplication
@EnableZuulProxy
@EnableEurekaClient
public class ZuulGatewayServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(ZuulGatewayServiceApplication.class, args);
	}

	@Bean
	public WebMvcConfigurer corsConfigurer() {
		return new WebMvcConfigurer() {
			@Override
			public void addCorsMappings(CorsRegistry registry) {
				registry.addMapping("/**").allowedOrigins("*");
			}
		};
	}
    }

* application.properties

```
spring.application.name=gateway-service
server.port=8765

zuul.ignored-headers=Access-Control-Allow-Credentials, Access-Control-Allow-Origin
#Pass the headers from gateway to sub-microservices.
zuul.sensitiveHeaders=Cookie,Set-Cookie

zuul.prefix=/api
#When path starts with /api/user/**, redirect it to user-service.
zuul.routes.user.path=/user/**
zuul.routes.user.serviceId=user-service
#When path starts with /api/course/**, redirect it to course-service.
zuul.routes.course.path=/course/**
zuul.routes.course.serviceId=course-service

#eureka
eureka.client.service-url.default-zone=http://localhost:8761/eureka/
#indicates the frequency the client sends heartbeats to indicate that it is still alive.
eureka.instance.lease-renewal-interval-in-seconds=30
#indicates the duration the server waits since it received the last heartbeat before it can evict an instance from its registry
eureka.instance.lease-expiration-duration-in-seconds=90

#load balancing
ribbon.eureka.enabled=true

#timeout
#this will help you load services eagerly. Otherwise for first time, we will get timeout exception.
zuul.ribbon.eager-load.enabled=true
#The read timeout in milliseconds. Default is 1000ms
ribbon.ReadTimeout=60000
#The Connection timeout in milliseconds. Default is 1000ms.
ribbon.ConnectTimeout=10000
```


##### .gitignore

```
# See http://help.github.com/ignore-files/ for more about ignoring files.

# compiled output
/dist
/tmp
/out-tsc
# Only exists if Bazel was run
/bazel-out

# dependencies
/node_modules

# profiling files
chrome-profiler-events.json
speed-measure-plugin.json

# IDEs and editors
/.idea
.c9/
*.launch
*.sublime-workspace

# IDE - VSCode
.vscode/*
!.vscode/settings.json
!.vscode/tasks.json
!.vscode/launch.json
!.vscode/extensions.json
.history/*

# misc
/.sass-cache
/connect.lock
/coverage
/libpeerconnection.log
npm-debug.log
yarn-error.log
testem.log
/typings

# System Files
.DS_Store
Thumbs.db

# Server
/.gradle
/build/
/!gradle/wrapper/gradle-wrapper.jar
/out/
/dist/
```

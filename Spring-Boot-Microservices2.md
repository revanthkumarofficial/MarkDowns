# SPRING BOOT MICRO SERVICES

##### Add Dependencies in Pom.xml
```
spring cloud config server
spring cloud starter netflix eureka client
<spring-cloud.version>Hoxton.SR3</spring-cloud.version>
```

##### Create Cloud Config Server Application
```
@SpringBootApplication
@EnableEurekaClient
@EnableConfigServer
public class CloudConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(CloudConfigServerApplication.class, args);
	}

}
```

##### application.yml
```
spring:
  application:
    name: CONFIG-SERVER
  cloud:
    config:
      server:
        git:
          uri: https://github.com/Java-Techie-jt/cloud-config-server

server:
  port: 9196

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8761/eureka/
  instance:
    hostname: localhost  
```

##### create cloud gateway

```
@SpringBootApplication
	@EnableEurekaClient
	@EnableHystrix
	public class CloudGatewayApplication {
	

		public static void main(String[] args) {
			SpringApplication.run(CloudGatewayApplication.class, args);
		}

	}
```

##### Create fallback controller
```
@RestController
public class FallbackController {

    @RequestMapping("/orderFallBack")
    public Mono<String> orderServiceFallBack() {
        return Mono.just("Order Service is taking too long to respond or is down. Please try again later");
    }
    @RequestMapping("/paymentFallback")
    public Mono<String> paymentServiceFallBack() {
        return Mono.just("Payment Service is taking too long to respond or is down. Please try again later");
    }
}
```

##### application.yml
```
spring:
  application:
    name: GATEWAY-SERVICE
  cloud:
    gateway:
      routes:
        - id: order-service
          uri: lb://ORDER-SERVICE
          predicates:
            - Path=/order/**
          filters:
            - name: CircuitBreaker
              args:
                name: order-service
                fallbackuri: forward:/orderFallBack
        - id: payment-service
          uri: lb://PAYMENT-SERVICE
          predicates:
            - Path=/payment/**
          filters:
            - name: CircuitBreaker
              args:
                name: payment-service
                fallbackuri: forward:/paymentFallback
                

server:
  port: 8989
  
management:
  endpoints:
    web:
      exposure:
        include: hystrix.stream

hystrix:
  command:
    fallbackcmd:
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 5000  
bootstrap.yml
spring:
  cloud:
    config:
      uri:
        - http://localhost:9196
```

##### create hystrix-dashboard

```
@SpringBootApplication
@EnableHystrixDashboard
public class HystrixDashboardApplication {

	public static void main(String[] args) {
		SpringApplication.run(HystrixDashboardApplication.class, args);
	}

}
```

##### application.properties
```
server.port=9195
```


##### order service
```
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment {

    private int paymentId;
    private String paymentStatus;
    private String transactionId;
    private int orderId;
    private double amount;
}

@Data
@AllArgsConstructor
@NoArgsConstructor
public class TransactionRequest {

    private Order order;
    private Payment payment;
}

@Data
@AllArgsConstructor
@NoArgsConstructor
public class TransactionResponse {
    private Order order;
    private double amount;
    private String transactionId;
    private String message;
}

@RestController
@RequestMapping("/order")
public class OrderController {

    @Autowired
    private OrderService service;
    
    @PostMapping("/bookOrder")
    public TransactionResponse bookOrder(@RequestBody TransactionRequest request) {
        return service.saveOrder(request);
    }
}

@Entity
@Table(name = "ORDER_TB")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Order {
    @Id
    private int id;
    private String name;
    private int qty;
    private double price;
}

public interface OrderRepository extends JpaRepository<Order,Integer> {
}

@Service
@RefreshScope
public class OrderService {

    Logger logger= LoggerFactory.getLogger(OrderService.class);
    @Autowired
    private OrderRepository repository;
    @Autowired
    @Lazy
    private RestTemplate template;

    @Value("${microservice.payment-service.endpoints.endpoint.uri}")
    private String ENDPOINT_URL;

    public TransactionResponse saveOrder(TransactionRequest request) throws JsonProcessingException {
        String response = "";
        Order order = request.getOrder();
        Payment payment = request.getPayment();
        payment.setOrderId(order.getId());
        payment.setAmount(order.getPrice());
        //rest call
        logger.info("Order-Service Request : "+new ObjectMapper().writeValueAsString(request));
        Payment paymentResponse = template.postForObject(ENDPOINT_URL, payment, Payment.class);

        response = paymentResponse.getPaymentStatus().equals("success") ? "payment processing successful and order placed" : "there is a failure in payment api , order added to cart";
        logger.info("Order Service getting Response from Payment-Service : "+new ObjectMapper().writeValueAsString(response));
        repository.save(order);
        return new TransactionResponse(order, paymentResponse.getAmount(), paymentResponse.getTransactionId(), response);
    }
}


@SpringBootApplication
@EnableEurekaClient
public class OrderServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(OrderServiceApplication.class, args);
	}
	 @Bean
	 @LoadBalanced
	public RestTemplate restTemplate(){
		return new RestTemplate();
	}
}


application.properties
server:
  port: 9192
spring:
  zipkin:
    base-url: http://localhost:9411/
  h2:
    console:
      enabled: true
  application:
    name: order-service
logging:
  file: C:/Users/Basant.Hota/Desktop/oms.log
  
  bootstrap.properties
  spring:
  cloud:
    config:
      uri:
        - http://localhost:9196
  

```


##### Payment Servcie

```
@RestController
@RequestMapping("/payment")
public class PaymentController {

    @Autowired
    private PaymentService service;

    @PostMapping("/doPayment")
    public Payment doPayment(@RequestBody Payment payment) {
        return service.doPayment(payment);
    }

    @GetMapping("/{orderId}")
    public Payment findPaymentHistoryByOrderId(@PathVariable int orderId){
              return service.findPaymentHistoryByOrderId(orderId);
    }


}

@Entity
@Table(name = "PAYMENT_TB")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Payment {
    @Id
    @GeneratedValue
    private int paymentId;
    private String paymentStatus;
    private String transactionId;
    private int orderId;
    private double amount;
}

public interface PaymentRepository extends JpaRepository<Payment,Integer> {
    Payment findByOrderId(int orderId);
}


@Service
public class PaymentService {
    
    @Autowired
    private PaymentRepository repository;
    
    Logger logger= LoggerFactory.getLogger(PaymentService.class);
    
    public Payment doPayment(Payment payment) throws JsonProcessingException {
        payment.setPaymentStatus(paymentProcessing());
        payment.setTransactionId(UUID.randomUUID().toString());
        logger.info("Payment-Service Request : {}",new ObjectMapper().writeValueAsString(payment));

        return repository.save(payment);
    }


    public String paymentProcessing(){
        //api should be 3rd party payment gateway (paypal,paytm...)
        return new Random().nextBoolean()?"success":"false";
    }


    public Payment findPaymentHistoryByOrderId(int orderId) {
        Payment payment=repository.findByOrderId(orderId);
        logger.info("paymentService findPaymentHistoryByOrderId : {}",new ObjectMapper().writeValueAsString(payment));
        return payment ;
    }
}



@SpringBootApplication
@EnableEurekaClient
public class PaymentServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(PaymentServiceApplication.class, args);
	}

}

application.yml

server:
  port: 9191
spring:
  zipkin:
    base-url: http://localhost:9411/
  h2:
    console:
      enabled: true
  application:
    name: payment-service
logging:
  file: C:/Users/Basant.Hota/Desktop/oms.log

bootstrap.yml
spring:
  cloud:
    config:
      uri:
        - http://localhost:9196


```

##### create service registry

```
SpringBootApplication
@EnableEurekaServer
public class ServiceRegistryApplication {

	public static void main(String[] args) {
		SpringApplication.run(ServiceRegistryApplication.class, args);
	}

}

application.yml
eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
server:
  port: 8761




```






















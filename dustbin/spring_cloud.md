# 01 服务注册与发现

## server端 

- 依赖包spring-cloud-starter-eureka-server

- @EnableEurekaServer

- ```yaml
  server:
  	port: 8761
  eureka:
  	client:
  		# 是否注册到其它eureka上去
  		registerWithEureka: false
  		# 是否要合并其它eureka上的数据
  		fetchRegistry: false
  		serviceUrl:
  			defaultZone: http://localhost:8761/eureka/
  			
  	
  	
  ```

## 服务端

- 依赖包spring-cloud-eureka

- @EnableDiscoveryClient / @EnableEurekaClient

- ```yaml
  eureka:
  	client:
  		serviceUrl:
  			defaultZone: http://localhost:8761/eureka
  	# 可以不配置，默认注册机器名到eureka,配置后使用ip		
  	instance:
      	prefer-ip-address: true
      	
  spring:
  	application:
  		name: microservice-provider-user
  ```

## 消费端 (ribbon负载均衡)

- 依赖包spring-cloud-starter-eureka

- @EnableDiscoveryClient

- ```java
  @Bean
  @LoadBalanced
  public RestTemplate restTemplate() {
      return new RestTemplate();
  }

  ....
  // Controller  
  @Autowired
  private RestTemplate restTemplate;

  @GetMapping("/user/{id}")
  public User findById(@PathVariable long id) {
    // 虚拟主机vip
  	return this.restTemplate.getForObject("http://microservice-provider-user" + id, User.class);    
  }

  @Autowired
  private LoadBalancerClient loadBalancerClient;

  @GetMapping("/log-user-instance")
  public void logUserInstance() {
      ServiceInstance serviceInstance = this.loadBalancerClient.choose("microservice-provider-user");
    	logger.info("{},{},{}", serviceInstance.getServiceId(), serviceInstance.getHost(), serviceInstance.getPort());
  }
  ```

- ```yaml
  eureka:
  	client:
  		serviceUrl:
  			defaultZone: http://localhost:8761/eureka
  spring:
  	application:
  		name: microservice-consumer-user
  ```

## 消费端feign (声明式http client)

- 集成spring-cloud-starter-feign, spring-cloud-starter-eureka

- @EnableDiscoveryClient, @EnableFeignClients

- ```java
  @FeignClient("microservice-provider-user")
  public interface UserClient {
      @GetMapping("/{id}")
  	public User findById(@PathVariable long id);  
  }

  @RestController
  public class UserControler {
      @Autowired
    	private UserClient userClient;
    
    	@GetMapping("/{id}")
    	public User findById(@PathVariable long id) {
          return userClient.findById(id);
      }
  }
  ```

- ```yaml
  spring:
  	application:
  		name: microservice-consumer-user-feign
  		
  eureka:
  	client:
  		serviceUrl:
  			defaultZone: http://localhost:8761/eureka
  ```



# 02 容错


































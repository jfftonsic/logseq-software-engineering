sources:: [Reference Doc](https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html), [How to Use Feign Client in Spring Boot](https://javatodev.com/how-to-use-feign-client-in-spring-boot/)

- Spring Cloud OpenFeign an openfeign integration module for spring boot.
- include `spring-cloud-starter-openfeign` along with `spring-cloud-dependencies` into our project.
- add `@EnableFeignClients` to your main class (which is annotated with `@SpringBootApplication`)
- example client:
	- ```java
	  @FeignClient(value = "instantwebtools-api", url = "https://api.instantwebtools.net/v1/")
	  // if using configuration from properties
	  //@FeignClient(value = "${app.feign.config.name}", url = "${app.feign.config.url}") 
	  public interface ApiClient {
	      @RequestMapping(method = RequestMethod.GET, value = "/airlines")
	      List<Airline> readAirLines();
	   
	      @RequestMapping(method = RequestMethod.GET, value = "/airlines/{airlineId}")
	      Airline readAirLineById(@PathVariable("airlineId") String airlineId);
	  }
	  ```
- example using the client in one rest controller method:
  collapsed:: true
	- ```java
	      private final ApiClient apiClient;
	      @GetMapping
	      public ResponseEntity readAirlineData (@RequestParam(required = false) String airlineId) {
	          if (airlineId == null) {
	              return ResponseEntity.ok(apiClient.readAirLines());
	          }
	          return ResponseEntity.ok(apiClient.readAirLineById(airlineId));
	  ```
- `application.properties` configurations
	- ```properties
	  app.feign.config.name=instantwebtools-api
	  app.feign.config.url=https://api.instantwebtools.net/v1/
	  ```
- Custom configurations
	- using custom clients e.g. [[OkHttp]] client which allows using HTTP/2
		- ```java
		  @Configuration
		  public class CustomFeignClientConfiguration {
		      
		      @Bean
		      public OkHttpClient client() {
		          return new OkHttpClient();
		      }
		      @Bean
		      Logger.Level feignLoggerLevel() {
		          return Logger.Level.FULL;
		      }
		  }
		  ```
		- then at the `@FeignClient` annotated class, pass the configuration parameter
			- ```java
			  @FeignClient(value = "${app.feign.config.name}", url = "${app.feign.config.url}", configuration = CustomFeignClientConfiguration.class)
			  ```
			-
- Setting Dynamic Headers into the Feign Client
	- in some cases we need to set dynamic headers like authentication tokens, basic auth credentials, auth identification, content type and etc, to a request we are sending to a 3rd party API.
	- ```java
	  @RequestMapping(method = RequestMethod.GET, value = "/airlines")
	      List<Airline> readAirLines(@RequestHeader("X-Auth-Token") String token);
	  ```
- Setting global headers to the feign client
  collapsed:: true
	- `feign.RequestInterceptor`
	- ```java
	  @Bean
	      public RequestInterceptor requestInterceptor() {
	          return requestTemplate -> {
	              requestTemplate.header("Accept", "application/json");
	              requestTemplate.header("Content-Type", "application/json");
	              requestTemplate.header("X-Auth-Code", "920d4d0b85443d98d86cb3c8c81d9eed");
	          };
	      }
	  ```
- Setting Feign Configurations Using Application Properties
  collapsed:: true
	- ```properties
	  # if global
	  feign.client.config.default.connect-timeout=20000
	  feign.client.config.default.read-timeout=20000
	  
	  # add client name to customize specific client
	  feign.client.config.instantwebtools-api.connect-timeout=20000
	  feign.client.config.instantwebtools-api.read-timeout=20000
	  ```
- Error handling
	- <mark style='background-color: red;'>PS: Connection refused doesn't reach this handler!!</mark>
	- `feign.codec.ErrorDecoder`
	- Example of customization
		- ```java
		  public class FeignCustomErrorDecoder implements ErrorDecoder {
		      @Override public Exception decode(String methodKey, Response response) {
		          switch (response.status()) {
		              case 400:
		                  log.error("Error in request went through feign client");
		                  //handle exception
		                  return new Exception("Bad Request Through Feign");
		              case 401:
		                  log.error("Error in request went through feign client");
		                  //handle exception
		                  return new Exception("Unauthorized Request Through Feign");
		              case 404:
		                  log.error("Error in request went through feign client");
		                  //handle exception
		                  return new Exception("Unidentified Request Through Feign");
		              default:
		                  log.error("Error in request went through feign client");
		                  //handle exception
		                  return new Exception("Common Feign Exception");
		          }
		      }
		  }
		  ```
		- then also add to the feign configuration (or put a `@Component` on the class above)
			- ```java
			  @Bean
			  public ErrorDecoder errorDecoder() { return new FeignCustomErrorDecoder();}
			  ```
	- Reading original error message from feign error decoder
		- example code:
			- ```java
			  @Slf4j
			  public class FeignCustomErrorDecoder implements ErrorDecoder {
			  
			  
			      @Override
			      public Exception decode(String methodKey, Response response) {
			          log.info("Is decoding error");
			  
			          //START DECODING ORIGINAL ERROR MESSAGE
			          String errorMessage = null;
			          Reader reader = null;
			          //capturing error message from response body.
			          try {
			              final var body = response.body();
			              if (body != null) {
			                  reader = body.asReader(StandardCharsets.UTF_8);
			                  String result = IOUtils.toString(reader);
			                  ObjectMapper mapper = new ObjectMapper();
			                  mapper.disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
			                  FeignException exceptionMessage = mapper.readValue(result,
			                          FeignException.class);
			                  errorMessage = exceptionMessage.getMessage();
			              } else {
			                  log.error("Null body");
			              }
			          } catch (IOException e) {
			              log.error("IO Exception on reading exception message feign client" + e);
			          } finally {
			              try {
			                  if (reader != null) {
			                      reader.close();
			                  }
			              } catch (IOException e) {
			                  log.error("IO Exception on reading exception message feign client" + e);
			              }
			          }
			          //END DECODING ORIGINAL ERROR MESSAGE
			  
			          switch (response.status()) {
			          case 400:
			              log.error("Error in request went through feign client httpStatus={} errorMessage={} ", response.status(), errorMessage);
			              //handle exception
			              return new Exception("Bad Request Through Feign");
			          case 401:
			              log.error("Error in request went through feign client httpStatus={} errorMessage={} ", response.status(), errorMessage);
			              //handle exception
			              return new Exception("Unauthorized Request Through Feign");
			          case 404:
			              log.error("Error in request went through feign client httpStatus={} errorMessage={} ", response.status(), errorMessage);
			              //handle exception
			              return new Exception("Unidentified Request Through Feign");
			          default:
			              log.error("Error in request went through feign client httpStatus={} errorMessage={} ", response.status(), errorMessage);
			              //handle exception
			              return new Exception("Common Feign Exception");
			          }
			      }
			  }
			  ```
- Feign client logging
  sources:: https://www.appsdeveloperblog.com/feign-client-logging/
	- Steps
		- Enable Logging in Application Properties File
			- ```properties
			  logging.level.<full class name of the interface here> = DEBUG
			  ```
		- Configure Feign Logger Level
			- `BASIC`, Log only the request method and URL and the response status code and execution time.
			  `HEADERS`, Log the basic information along with request and response headers.
			  `FULL`, Log the headers, body, and metadata for both requests and responses.
			- ```java
			  @SpringBootApplication
			  @EnableDiscoveryClient
			  @EnableFeignClients
			  public class PhotoAppApiApplication {
			  
			      public static void main(String[] args) {
			          SpringApplication.run(PhotoAppApiApplication.class, args);
			      }
			  
			      @Bean
			      Logger.Level feignLoggerLevel() {
			          return Logger.Level.FULL;
			      }
			  }
			  ```
-
-
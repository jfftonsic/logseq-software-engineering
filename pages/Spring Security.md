sources:: [Baeldung - Spring @EnableWebSecurity vs. @EnableGlobalMethodSecurity](https://www.baeldung.com/spring-enablewebsecurity-vs-enableglobalmethodsecurity), [EnableWebSecurity reference docs](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/web/configuration/EnableWebSecurity.html), [GlobalMethodSecurityConfiguration reference docs](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/method/configuration/GlobalMethodSecurityConfiguration.html), [Spring Security Method Level Annotations Example](https://memorynotfound.com/spring-security-method-level-annotations-example/)

- Customizing security
	- `@EnableWebSecurity`
	  collapsed:: true
		- With Spring Security on the classpath, Spring Boot Security Auto-Configuration‘s `WebSecurityEnablerConfiguration` activates `@EnableWebSecurity` for us.
			- Default security activates both HTTP security filters and the security filter chain and applies basic authentication to our endpoints.
		- With
		  ```java
		  @EnableWebSecurity
		  public class MySecurityConfigurer extends WebSecurityConfigurerAdapter {
		    	@Override
		  	protected void configure(HttpSecurity http) {
		        	// or
		      	http.authorizeRequests((requests) -> requests.anyRequest().authenticated());
		      	http.formLogin();
		      	http.httpBasic();
		        
		        	// or
		        	http.authorizeRequests().anyRequest().authenticated()
		    			.and().formLogin()
		    			.and().httpBasic()
		            
		          // Require Users to Have an Appropriate Role
		          http.authorizeRequests()
		          	.antMatchers("/admin/**")
		          	.hasRole("ADMIN")
		          	.antMatchers("/protected/**")
		          	.hasRole("USER");
		        
		  	}
		    
		    @Override
		    public void configure(WebSecurity web) {
		      //Relax Security for Public Resources
		        web.ignoring().antMatchers("/hello/*");
		    }
		  }
		       
		  ```
		  #sample #[[Spring Security]] #Security 
		  we get the benefits of Spring Security's other defenses while also being able to add customizations.
	- `@EnableGlobalMethodSecurity`
		- #+BEGIN_CAUTION
		  Annotated security only gets applied when we enter a class via a public method.
		  #+END_CAUTION
		- code sample
			- ```java
			  // securedEnabled – enables the spring @Secured annotation.
			  // jsr250Enabled – enables the JSR-250 standard java security annotations.
			  // prePostEnabled – enables the spring @PreAuthorize and PostAuthorize annotations.
			  @EnableGlobalMethodSecurity(jsr250Enabled = true)
			  @Controller
			  public class AnnotationSecuredController {
			      @RolesAllowed("ADMIN")
			      @RequestMapping("/admin")
			      public String adminHello() {
			          return "Hello Admin";
			      }
			  
			      @RolesAllowed("USER")
			      @RequestMapping("/protected")
			      public String jsr250Hello() {
			          return "Hello Jsr250";
			      }
			  }
			  
			  // and to allow access to public resources
			  @Configuration
			  public class MyWebConfig {
			      @Bean
			      public WebSecurityCustomizer ignoreResources() {
			          return (webSecurity) -> webSecurity
			            .ignoring()
			            .antMatchers("/hello/*");
			      }
			  }
			  ```
			  #sample #Security #[[Spring Security]]
		- More advanced configurations may wish to extend [GlobalMethodSecurityConfiguration](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/method/configuration/GlobalMethodSecurityConfiguration.html) and override the protected methods to provide custom implementations.
			- #+BEGIN_IMPORTANT
			  Note that [EnableGlobalMethodSecurity](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/method/configuration/EnableGlobalMethodSecurity.html) still must be included on the class extending [GlobalMethodSecurityConfiguration](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/config/annotation/method/configuration/GlobalMethodSecurityConfiguration.html) to determine the settings.
			  #+END_IMPORTANT
- testing:
	- Testing via Web Requests
	  collapsed:: true
		- When testing an implementation that did not use `@EnableGlobalMethodSecurity`
		  ```java
		  @RunWith(SpringRunner.class)
		  @SpringBootTest(webEnvironment = RANDOM_PORT)
		  public class WebSecuritySpringBootIntegrationTest {
		    @Autowired
		    private TestRestTemplate template;
		  - @Test
		  	public void givenPublicResource_whenGetViaWeb_thenOk() {
		  ResponseEntity<String> result = template.getForEntity("/hello/baeldung.txt", String.class);
		  assertEquals("Hello From Baeldung", result.getBody());
		  	}
		  
		  	@Test
		  	public void whenGetProtectedViaWeb_thenForbidden() {
		  //Here we get a FORBIDDEN response, as our anonymous request does not have the required role.
		  ResponseEntity<String> result = template.getForEntity("/protected", String.class);
		  assertEquals(HttpStatus.FORBIDDEN, result.getStatusCode());
		  	}
		  }
		  ```
		  #Spring #[[Spring Security]] #testing #junit #security
	- Testing via Auto-Wiring and Annotations
	  collapsed:: true
		- When testing an implementation that used `@EnableGlobalMethodSecurity`
		  ```java
		  @RunWith(SpringRunner.class)
		  @SpringBootTest(webEnvironment = RANDOM_PORT)
		  public class GlobalMethodSpringBootIntegrationTest {
		    @Autowired
		    private AnnotationSecuredController api;
		  - // testing our publicly accessible method using @WithAnonymousUser
		  	@Test
		  	@WithAnonymousUser
		  	public void givenAnonymousUser_whenPublic_thenOk() {
		  assertThat(api.publicHello()).isEqualTo(HELLO_PUBLIC);
		  	}
		  
		  	// use @WithMockUser annotations to access our protected methods.
		  
		  	// test our JSR-250 protected method with a user that has the USER role
		  	@WithMockUser(username="baeldung", roles = "USER")
		  	@Test
		  	public void givenUserWithRole_whenJsr250_thenOk() {
		  assertThat(api.jsr250Hello()).isEqualTo("Hello Jsr250");
		  	}
		  
		  	//attempt to access the same method when our user doesn't have the right role
		  	@WithMockUser(username="baeldung", roles = "NOT-USER")
		  	@Test(expected = AccessDeniedException.class)
		  	public void givenWrongRole_whenJsr250_thenAccessDenied() {
		  api.jsr250Hello();
		  	}
		  }
		  ```
		  #Spring #[[Spring Security]] #testing #junit #security
- when we want to **enforce security based on whether a domain object is owned by the user**, we can use [Spring Security Access Control Lists](https://www.baeldung.com/spring-security-acl).
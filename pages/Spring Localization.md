sources:: https://easylokal.com/blog/a-complete-step-by-step-guide-to-do-localization-in-spring-boot-java/
tags:: Spring

-
- `MessageSource`
	- can be autowired
	- resolves the messages based on the key and locale provided
- Language resources
	- properties-style files located on the `resources` folder and follow a nomenclature, for example:
		- messages.properties: contains default language strings, English by convention
		- messages_en.properties: contains English strings
		- messages_de.properties: contains German strings
- From where to get the locale?
	- from [[Spring]]'s `WebRequest` <span style="color: red; background-color: black; font-weight: bold">(I personally don't know yet if this is something that shouldn't be done)</span>
	  collapsed:: true
		- ```java
		  localeHelper.localizeMessage(ERROR_REQUEST_NOT_READABLE, request.getLocale());
		  ```
	- from [[Spring]]'s `LocaleResolver`
	  collapsed:: true
		- `FixedLocaleResolver`
			- Always resolves the locale to a singular fixed language mentioned in the project properties.
			- Mostly used for debugging purposes.
		- `AcceptHeaderLocaleResolver`
			- Resolves the locale using an “accept-language” HTTP header retrieved from an HTTP request.
		- `SessionLocaleResolver`
			- Resolves the locale and stores it in the HttpSession of the user.
			- The resolved locale data is persisted only for as long as the session is live.
		- `CookieLocaleResolver`
			- Resolves the locale and stores it in a cookie stored on the user’s machine.
			- as long as the user doesn’t clear the browser cookies, the resolved locale data will last even between sessions.
	- If your application has user-level language configuration
	  collapsed:: true
		- save language code against a user and fetch the language code from user data
- To detect when the locale changes
	- add a `LocaleChangeInterceptor`
- Best practices
	- Create an enum (or enums) to hold your message keys
	  collapsed:: true
		- ```java
		  @Getter
		  @RequiredArgsConstructor
		  public enum LocaleStringKey {
		      ERROR_REQUEST_NOT_READABLE("api.error.request-not-readable");
		  
		      private final String value;
		  }
		  ```
	- Create a helper bean to improve code readability
	  collapsed:: true
		- ```java
		  
		  @Component
		  @RequiredArgsConstructor(onConstructor = @__(@Autowired))
		  @FieldDefaults(level = AccessLevel.PRIVATE, makeFinal = true)
		  @Slf4j
		  public class LocaleHelper {
		      private MessageSource messageSource;
		  
		      public String localizeMessage(LocaleStringKey key, String[] paramList) {
		          return localizeMessage(key, paramList, null);
		      }
		  
		      public String localizeMessage(LocaleStringKey key, String[] paramList, Locale locale) {
		          return this.messageSource.getMessage(key.getValue(), paramList, locale);
		      }
		  
		      public String localizeMessage(LocaleStringKey key) {
		          return localizeMessage(key, (Locale) null);
		      }
		  
		      public String localizeMessage(LocaleStringKey key, Locale locale) {
		          return this.messageSource.getMessage(key.getValue(), new String[] {}, locale);
		      }
		  }
		  
		  ```
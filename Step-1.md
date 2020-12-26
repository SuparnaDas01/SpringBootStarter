## Project Setup

- Generate project using the startup configuration from this site:
  https://start.spring.io/
  Select dependency as Web and DevTools
  
 -Extract the zip and import the project as Maven project
 
 -Add "LoginController" class with following:
 
 ## com.web.springbootwebapp.controller
 ``` 
 @Controller
public class LoginController {

	@RequestMapping("/login")
	@ResponseBody
	public String loginMessage() {
		
		return "Hello App";
	}
```

## src/main/resources/application.properties
```
logging.level.org.springframework.web: DEBUG
```
## Run the app
This triggers the spring application and starts the embedded tomcat server
Navigate to http://localhost:8080/login

It should show Hello App

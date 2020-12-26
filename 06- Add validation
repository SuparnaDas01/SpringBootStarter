
## What You Will Learn during this Step:
- Add validation for userid and password
- Create Bean called LoginService and reference it using @Component
- Add it as dependency in LoginController using @Autowired annotation


### src/main/java/com.web.springbootwebapp.service.LoginService.java
```
public class LoginService {

	public boolean validateUser(String userID,String password) {
		
		//Create hardcoded usrid and password
		
		return userID.equalsIgnoreCase("Ravi") && password.equalsIgnoreCase("Dummy");
		
	}
}
```

### src/main/java/com.web.springbootwebapp.controller/LoginController.java
```
@Controller
public class LoginController {

	@Autowired
	LoginService service;
	
	@RequestMapping(value="/login",method= RequestMethod.GET)	
	public String showLoginPage(ModelMap model){		
		return "login";
	}
	
	@RequestMapping(value="/login",method= RequestMethod.POST)	
	public String showWelcomePage(ModelMap model,@RequestParam String name, @RequestParam String password){
		
    boolean isValidUser = service.validateUser(name, password);
		
		if(!isValidUser) {
			model.put("errorMessage", "InvalidUser");
			return "login";
		}
		model.put("name", name);
		model.put("password", password);
		
		return "welcome";
	}
``` 
### src/main/webapp/WEB-INF/jsp/login.jsp
```
<html>
<head>
<title>First Web Application</title>
</head>
<body>
<font color="red">${errorMessage}</font>
<form method="POST">
 Name : <input name="name" type="text"/>
 Password : <input name="password" type="password"/>
<input type="submit"/>
</form>
</body>
</html>
```


### src/main/webapp/WEB-INF/jsp/welcome.jsp
```
<html>
<head>
<title>First Web Application</title>
</head>
<body>
Welcome ${name}!!. Your password is ${password}
</body>
</html>
```
Summary

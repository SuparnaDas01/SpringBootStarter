## Use of Request Parameter and using it to query parameters in GET Request

### Add Request Param and Model to the LoginController class
```
package com.web.springbootwebapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class LoginController {
	
	//Model
	
	@RequestMapping("/login")
	public String loginMessage(@RequestParam String name, ModelMap model){
		model.put("name", name);
		return "login";
	}
}
```
### Update JSP Page to derive name value

src/main/webapp/WEB-INF/jsp/login.jsp
```
<html>

<head>
<title>First Web Application</title>
</head>

<body>
My First JSP!! Welcome ${name}!
</body>
</html>
```

### Restart server and query the following url
```
http://localhost:8080/login?name=%22Joly%22
```

## Add the code snippets to the login jsp

```
<html>
<head>
<title>First Web Application</title>
</head>
<body>
<form method="POST">
 Name : <input name="name" type="text"/>
 Password : <input name="password" type="password"/>
<input type="submit"/>
</form>
</body>
</html>
```

### Create welcome.jsp page and add the below snippet
```
<html>
<head>
<title>First Web Application</title>
</head>
<body>
Welcome ${name}!!
</body>
</html>
```

### Update the request mapping for GET and POST Request
```
package com.web.springbootwebapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;


@Controller
public class LoginController {

	@RequestMapping(value="/login",method= RequestMethod.GET)	
	public String showLogin(ModelMap model){		
		return "login";
	}
	
	@RequestMapping(value="/login",method= RequestMethod.POST)	
	public String handleLogin(@RequestParam String name, ModelMap model){
		model.put("name", name);
		return "welcome";
	}
	
}
```

### Restart server and note the changes on entering the field values
```
http://localhost:8080/login
```

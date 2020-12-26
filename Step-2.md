## Creating the first JSP Page

/src/main/webapp/WEB-INF/jsp/login.jsp

```
<html>
<head>
<title>Yahoo!!</title>
</head>
<body>
My First JSP!!!
</body>
</html>
```

## Add following dependecy To enable jsp support in embedded tomcat server!
```
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <scope>provided</scope>
        </dependency>
```

## /src/main/resources/application.properties
```
spring.mvc.view.prefix: /WEB-INF/jsp/
spring.mvc.view.suffix: .jsp
logging.level.: DEBUG
```

## src/main/java/com.web.springbootwebapp.controller/LoginController.java
package com.in28minutes.springboot.web.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class LoginController {
	
	@RequestMapping("/login")
	public String loginMessage(){
		return "login";
	}
}


### Problem Statement
- value of Model 'name' which was created in LoginController does not persist when the /list-todos page is fetched.
- In order to solve this problem, concept of session management is used.
- Scope of Session Vs Model Vs Request
- Scope of Request parameter e.g login and password is only available for that Request(/login), same applies for values in model which will not be available for other request
- If you want value to persists across multiple request, then use session
- How to put value to session. By using annotaion @SessionAttributes(name of model attribute which need to be accessed)

#### Add @SessionAttributes("name") in "TodoController" and "LoginController"
```
@SessionAttributes("name")
```
#### TodoController.java
```
package com.web.springbootwebapp.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.SessionAttributes;

import com.web.springbootwebapp.service.TodoService;


@Controller
@SessionAttributes("name")
public class TodoController {

	@Autowired
	TodoService service;
	
	@RequestMapping(value="/list-todos",method= RequestMethod.GET)	
	public String showTodos(ModelMap model){	
		
		String name = (String)model.get("name");
		model.put("todos",service.retrieveTodos(name));
		//model.put("todos",service.retrieveTodos("in28Minutes"));
		return "list-todos";
	}
	
	
	
}

```
###
Https are stateless protocol , they do not store anything. But session are conversational storage, meaning they persist across requests.

Now you should be able to login and navigate till todos and values of name will still persist.

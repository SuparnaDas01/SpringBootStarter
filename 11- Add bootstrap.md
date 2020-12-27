#### What we will do:
-Add bootstrap to give basic formatting to the page : We use bootstrap classes container,table and table-striped.
-We will use webjars
-Already auto configured by Spring Boot : o.s.w.s.handler.SimpleUrlHandlerMapping : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]

#### Useful Snippets
```
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>bootstrap</artifactId>
            <version>3.3.6</version>
        </dependency>
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>1.9.1</version>
        </dependency>
    ************Add in list-todos.jsp***************    
        <script src="webjars/jquery/1.9.1/jquery.min.js"></script>
	    <script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
		<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
	    		rel="stylesheet">
```          
#### src/main/webapp/WEB-INF/jsp/list-todos.jsp
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<html>

<head>
<title>Todo's for ${name}</title>
<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
	rel="stylesheet">
</head>

<body>
	<div class="container">
		<table class="table table-striped">
			<caption>Your todos are</caption>
			<thead>
				<tr>
					<th>Description</th>
					<th>Target Date</th>
					<th>Is it Done?</th>
				</tr>
			</thead>
			<tbody>
				<c:forEach items="${todos}" var="todo">
					<tr>
						<td>${todo.desc}</td>
						<td>${todo.targetDate}</td>
						<td>${todo.done}</td>
					</tr>
				</c:forEach>
			</tbody>
		</table>
		<div> <a class="button" href="/add-todo">Add a Todo</a></div>

		<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
		<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
	</div>
</body>

</html>
```

### Delete Todos
Add functionality to delete a todo in "list-todos.jsp"
```
<a type="button" class="btn btn-warning" 
		href="/delete-todo?id=${todo.id}">Delete</a>
```
#### Add delete todo method
```
package com.web.springbootwebapp.controller;

import java.util.Date;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.SessionAttributes;

import com.web.springbootwebapp.service.TodoService;


@Controller
@SessionAttributes("name")
public class TodoController {

	@Autowired
	TodoService service;
	
	@RequestMapping(value="/list-todos",method= RequestMethod.GET)	
	public String showTodos(ModelMap model){			
		String name = (String) model.get("name");
		model.put("todos",service.retrieveTodos(name));
		//model.put("todos",service.retrieveTodos("in28Minutes"));
		return "list-todos";
	}
	
	@RequestMapping(value="/add-todo",method= RequestMethod.GET)	
	public String showAddTodoPage(ModelMap model){				
		return "todo";
	}
	
	@RequestMapping(value="/delete-todo",method= RequestMethod.GET)	
	public String deleteTodo(@RequestParam int id){			
		service.deleteTodo(id);
		return "redirect:/list-todos";
	}	
	
	@RequestMapping(value="/add-todo", method = RequestMethod.POST)
	public String addTodo(ModelMap model, @RequestParam String desc){
		service.addTodo((String) model.get("name"), desc, new Date(), false);
		return "redirect:/list-todos";
	}
```	

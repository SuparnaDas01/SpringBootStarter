### What we will do:
Add Facility to add New Todo
todo.jsp
Importance of redirect:/list-todos

#### in list-todos.jsp add facility to add the todo
```
<html>
<head>
<title>To Do's Application</title>
</head>
<body>
	Here are the List of Todo's:
	${todos}
	<BR/>
	<a href="/add-todo">Add a Todo</a>
</body>

</html>
```
#### create todo.jsp
```
<html>
<head>
<title>Add To Do's Application</title>
</head>
<body>
	This is a add Todo
	
</body>

</html>
```
#### Add request for GET and POST for the todo.jsp-src/main/java/com.web.springbootwebapp.controller/TodoController.java

```
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
	
	@RequestMapping(value="/add-todo", method = RequestMethod.POST)
	public String addTodo(ModelMap model, @RequestParam String desc){
		service.addTodo((String) model.get("name"), desc, new Date(), false);
		return "redirect:/list-todos";
	}
	
}
```
#### create a form in todo.jsp to capture details from the user
```
<html>
<head>
<title>Add To Do's Application</title>
</head>
<body>
	ADD Todo Page for ${name}
	<form method="post">
	Description: <input name="desc" type="text"/>
					<input type="submit"/>	
	
	</form>
	
</body>

</html>
```


#### src/main/webapp/WEB-INF/jsp/list-todos.jsp
- Add th tag for update and button class for update
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
	<H1>Your Todo's</H1>
	<table class="table table-striped">
	<Caption>Your Todo's are</Caption>
	<thead>
		<tr>
		<th>Description</th>
		<th>Target Date</th>
		<th>Is It Done?</th>
		<th></th>
		<th></th>
		</tr>
	</thead>
	<tbody>
		<c:forEach items="${todos}" var="todo">		
		
		<tr>
		<td>${todo.desc}</td>
		<td>${todo.targetDate}</td>
		<td>${todo.done}</td>
    <td><a type="button" class="btn btn-success" href="/update-todo?id=${todo.id}">Update</a></td>
		<td><a type="button" class="btn btn-warning" href="/delete-todo?id=${todo.id}">Delete</a></td>
		
		
		</tr>
		</c:forEach>
	</tbody>
	</table>
			<div> <a class="button" href="/add-todo">Add a Todo</a></div>
	</div>
	<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
	<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
	
</body>

</html>
```
#### Update Todo button is non functional - Add a method in todocontroller for update
- Create updateTodo method. We do not have retriveTodo method to fetch the value of existing service.
- Go to "todoService" and create "retrieveTodo" method
- Then call it inside updateTodo method using below line:
service.retrieveTodo(id)
- Next we want to display this in screen so we will use model(passing data from Controller to view - using modelMap)
-Once details are added send user to "todo" page
```
TodoService.java
********************
public Todo retrieveTodo(int id) {
		for(Todo todo: todos) {
			if (todo.getId()==id) {
				return todo;
			}
		}
    	
    	return null;    	
    }
    
    public void updateTodo(Todo todo) {
    	todos.remove(todo);
    	todos.add(todo);
    }
    
TodoController.java
********************
@RequestMapping(value="/update-todo",method= RequestMethod.GET)	
	public String showUpdateTodoPage(@RequestParam int id,ModelMap model){			
		Todo todo = service.retrieveTodo(id);
		model.put("todo", todo);
		return "todo";
	}
	
  Once user clicks on update button, we will want to display the current todo. These 2 lines shows current todo:
  Todo todo = service.retrieveTodo(id);
		model.put("todo", todo);
  Once we have current todo we want to take user todo.jsp
  ```
  - Once we are in todo page if user updates and clicks on "Add" button application will crash since we created GET for viewing the update. 
  - But We need to implement the Post request for posting our updates
  ```
  @RequestMapping(value="/update-todo",method= RequestMethod.POST)	
	public String updateTodo(@Valid Todo todo, BindingResult result){			
		if(result.hasErrors()){
			return "todo";
		}
	  service.updateTodo(todo);
	return "redirect:/list-todos";
	}
  ```
  #### At this point when trying to update, system will throw Internal-500 Error resulting in Null pointer exception.
  Earlier only desc was mapped and not ID. So while doing update it is only finding desc
  Add following snippet to todo.jsp, since it is unable to interpret newly added update based on ID. 
  ```
  <form:hidden path="id"/> 
  ```
 Also Set user detail in update todo method. In order to pick it from session ...add modelmap and get value using model.getname
  ```
  @RequestMapping(value="/update-todo",method= RequestMethod.POST)	
	public String updateTodo(ModelMap model,@Valid Todo todo, BindingResult result){	

		if(result.hasErrors()){
			return "todo";
		}
    		todo.setUser((String) model.get("name"));
	  service.updateTodo(todo);
	return "redirect:/list-todos";
	}
  ```
  - At this point when application is run again, Update is successful however target date field will be set as blank for updated field


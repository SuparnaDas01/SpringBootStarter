### What we will do:Part-1
- Lets use a command bean for Todo
- Add Server side Validations
- The JSR 303 and JSR 349 defines specification for the Bean Validation API (version 1.0 and 1.1, respectively), and Hibernate Validator is the reference implementation.
org.hibernate:hibernate-validator

- In TodoController.java Instead of using @RequestParam String desc for different fields. we can use command bean Todo todo. By this we can directly call desc from Todo bean. 
- This concept is called Form backing object  or command bean. Map value directly from bean to form and viseversa 
- Once bean is created we can add validation(2 way binding) and then finally enable them in Controller
- Update todocontroller to enable command bean
- Enable spring form tag in view so that both side binding can happen

Implementing Server Side Validation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Command Bean or Form Backing Bean

Add Validation
Use Validation on Controller
Display Errors in View

#### Snippets updated
```
TodoController.java

@RequestMapping(value="/add-todo",method= RequestMethod.GET)	
	public String showAddTodoPage(ModelMap model){		
		//added as part of form validation
		model.addAttribute("todo",new Todo(0,(String) model.get("name"),"",new Date(),false));
		return "todo";
	}
  
  @RequestMapping(value="/add-todo", method = RequestMethod.POST)
	public String addTodo(ModelMap model, Todo todo){
		service.addTodo((String) model.get("name"), todo.getDesc(), new Date(), false);
		return "redirect:/list-todos";
	}
  
  ```
  #### Todo.jsp
  ```
  <%@taglib uri="http://www.springframework.org/tags/form" prefix="form"%>
<html>

<head>
<title>First Web Application</title>
<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
	rel="stylesheet">

</head>

<body>
	<div class="container">
		<form:form method="post">
			<form:form method="post" modelAttribute="todo">
				<form:label path="desc">Description</form:label> 
				<form:input path="desc" type="text" class="form-control" required="required"/>
			</fieldset>

			<button type="submit" class="btn btn-success">Add</button>
		</form:form>
	</div>

	<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
	<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>

</body>

</html>
Please note: commandName is depricated. Use modelAttribute instead
https://stackoverflow.com/questions/58805047/unable-to-find-setter-method-for-attribute-error-in-spring-boot

<form:form method="post" modelAttribute="todo"> binds the service.addTodo((String) model.get("name"), todo.getDesc(), new Date(), false); in todocontroller
```
#### Run the application and ensure it runs without error
#### Part 2- Using JSR 349 validations
- Add validation that user should enter atleast 6 characters in the description field in order to add Todo
- IF valdiation fails then redirect to the add-todo 
- Add Javax valdiation dependency
- Add bean validation API in todo.java over description field -@Size(min = 10, message = "Enter atleast 10 Characters.")

#### Snippets:
```
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>2.0.1.Final</version>
</dependency>
```
#### Enable binding by adding validation in the todocontroller by adding @Valid and BindingResult(to determine if validation succeded or failed) from Javax validations
```
@RequestMapping(value = "/add-todo", method = RequestMethod.POST)
	public String addTodo(ModelMap model, @Valid Todo todo, BindingResult result) {
		
		if(result.hasErrors()){
			return "todo";
		}
		
		service.addTodo((String) model.get("name"), todo.getDesc(), new Date(),
				false);
		return "redirect:/list-todos";
	}
```  
#### In order to show error add this line in the todo.jsp to show error for description field
```
<form:errors path="desc" cssClass="text-warning"/>
```


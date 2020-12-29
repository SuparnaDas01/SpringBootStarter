### What we will do
- Add functionality to add Target date and update Target date in todo form
- initBinder method

- Add form group for target date in todo.jsp
```

```
-On trying to update target date...following error is encountered: Since it is trying to bind this field with target date field but their format is different
Failed to convert property value of type java.lang.String to required type java.util.Date for property targetDate; nested exception is org.springframework.core.convert.ConversionFailedException: 
Failed to convert from type [java.lang.String] to type [java.util.Date] for value 89900; nested exception is java.lang.IllegalArgumentException

-First thing we need to do is to set common date format across the application. 
-We can do it with help of initbinder
- Add initBinder to "TodoController.java" and specify the date format
```
@InitBinder
	protected void initBinder(WebDataBinder binder) {
		SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
		binder.registerCustomEditor(Date.class, new CustomDateEditor(
				dateFormat, false));
	}
 ``` 
 - At this point when application is run,target date can be updated successfully.
 -Howver to make target date same across screens following changes are made:
 - Add date formatter in list-todo.jsp(only for formatting ,no binding happening here)
 <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
 
 - Replace ths line <td>${todo.targetDate}</td> with below line to have consistent format across screens
		<fmt:formatDate pattern="dd/MM/yyyy" value="${todo.targetDate}" />
 -"TodoController.java" under addTodo method replace newDate() in following line: with todo.getTartgetDate()
 service.addTodo((String) model.get("name"), todo.getDesc(), new Date(), false);
 with following:
 service.addTodo((String) model.get("name"), todo.getDesc(), todo.getTargetDate(), false);
 
 Above steps fixes the TargetDate field format in both Add and update functionality
 
 Run the application to see if it works with success
 
 #### list-tod.jsp
 ```
 <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>


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
		<td><fmt:formatDate value="${todo.targetDate}" pattern="dd/MM/yyyy"/></td>
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
### todo.jsp
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
		<form:form method="post" modelAttribute="todo">
		<form:hidden path="id"/> 
			<fieldset class="form-group">
				<form:label path="desc">Description</form:label> 
				<form:input path="desc" type="text" class="form-control" required="required"/>
				<form:errors path="desc" cssClass="text-warning"/>
			</fieldset>			
				
			<fieldset class="form-group">
				<form:label path="targetDate">Target Date</form:label> 
				<form:input path="targetDate" type="text" class="form-control" required="required"/>
				<form:errors path="targetDate" cssClass="text-warning"/>
			</fieldset>

		<button type="submit" class="btn btn-success">Add</button>
		</form:form>
	</div>

	<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
	<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>

</body>

</html>
```
### Add Date picker functionality using bootstrap webjars
```
<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap-datepicker</artifactId>
			<version>1.0.1</version>
		</dependency>
```
Snippers:
<script src="webjars/bootstrap-datepicker/1.0.1/js/bootstrap-datepicker.js"></script>
	<script>
		$('#targetDate').datepicker({
			format : 'dd/mm/yyyy'
		});
	</script>
  
  Add in todo.jsp
  ```
  <%@taglib uri="http://www.springframework.org/tags/form" prefix="form"%>
<html>

<head>
<title>First Web Application</title>
<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet">

</head>

<body>
	<div class="container">
		<form:form method="post" modelAttribute="todo">
		<form:hidden path="id"/> 
			<fieldset class="form-group">
				<form:label path="desc">Description</form:label> 
				<form:input path="desc" type="text" class="form-control" required="required"/>
				<form:errors path="desc" cssClass="text-warning"/>
			</fieldset>			
				
			<fieldset class="form-group">
				<form:label path="targetDate">Target Date</form:label> 
				<form:input path="targetDate" type="text" class="form-control" required="required"/>
				<form:errors path="targetDate" cssClass="text-warning"/>
			</fieldset>

		<button type="submit" class="btn btn-success">Add</button>
		</form:form>
	</div>

	<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
	<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
	<script src="webjars/bootstrap-datepicker/1.0.1/js/bootstrap-datepicker.js"></script>
	<script>
		$('#targetDate').datepicker({
			format : 'dd/mm/yyyy'
		});
	</script>

</body>

</html>
```

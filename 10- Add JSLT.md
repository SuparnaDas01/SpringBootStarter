### What we will do:
- Display Todos in a table using JSTL Tags
- <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
- Add Dependency for jstl

#### Add support for JSLT
```
<dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
        </dependency>
```
#### update list-todos.jsp
```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<html>
<head>
<title>Todo's for ${name}</title>
</head>
<body>
	<H1>Your Todo's</H1>
	<table>
	<Caption>Your Todo's are</Caption>
	<thead>
		<tr>
		<th>Description</th>
		<th>Target Date</th>
		<th>Is It Done?</th>
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
	
	
</body>

</html>
```

#### Run the application and verify it works with success

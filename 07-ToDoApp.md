
## What we will do:
Create TodoController and list-todos.jsp
Make TodoService a @Service and inject it


### Snippet - /src/main/java/com.web.springbootwebapp/model/Todo.java
```
package com.in28minutes.springboot.web.model;

import java.util.Date;

public class Todo {
    private int id;
    private String user;
    private String desc;
    private Date targetDate;
    private boolean isDone;

    public Todo(int id, String user, String desc, Date targetDate,
            boolean isDone) {
        super();
        this.id = id;
        this.user = user;
        this.desc = desc;
        this.targetDate = targetDate;
        this.isDone = isDone;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUser() {
        return user;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public Date getTargetDate() {
        return targetDate;
    }

    public void setTargetDate(Date targetDate) {
        this.targetDate = targetDate;
    }

    public boolean isDone() {
        return isDone;
    }

    public void setDone(boolean isDone) {
        this.isDone = isDone;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + id;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (getClass() != obj.getClass()) {
            return false;
        }
        Todo other = (Todo) obj;
        if (id != other.id) {
            return false;
        }
        return true;
    }

    @Override
    public String toString() {
        return String.format(
                "Todo [id=%s, user=%s, desc=%s, targetDate=%s, isDone=%s]", id,
                user, desc, targetDate, isDone);
    }

}
```

## /src/main/java/com.web.springbootwebapp.service/TodoService.java

```
package com.web.springbootwebapp.service;

import java.util.ArrayList;
import java.util.Date;
import java.util.Iterator;
import java.util.List;

import org.springframework.stereotype.Service;

import com.web.springbootwebapp.model.Todo;

@Service
public class TodoService {
    private static List<Todo> todos = new ArrayList<Todo>();
    private static int todoCount = 3;

    static {
        todos.add(new Todo(1, "in28Minutes", "Learn Spring MVC", new Date(),
                false));
        todos.add(new Todo(2, "in28Minutes", "Learn Struts", new Date(), false));
        todos.add(new Todo(3, "in28Minutes", "Learn Hibernate", new Date(),
                false));
    }

    public List<Todo> retrieveTodos(String user) {
        List<Todo> filteredTodos = new ArrayList<Todo>();
        for (Todo todo : todos) {
            if (todo.getUser().equals(user)) {
                filteredTodos.add(todo);
            }
        }
        return filteredTodos;
    }

    public void addTodo(String name, String desc, Date targetDate,
            boolean isDone) {
        todos.add(new Todo(++todoCount, name, desc, targetDate, isDone));
    }

    public void deleteTodo(int id) {
        Iterator<Todo> iterator = todos.iterator();
        while (iterator.hasNext()) {
            Todo todo = iterator.next();
            if (todo.getId() == id) {
                iterator.remove();
            }
        }
    }



}
```

### JSP changes
welcome.jsp - Once user logs in to the application, we want to show him a welcome message to click on a ToDo link

```
<html>
<head>
<title>First Web Application</title>
</head>
<body>
Welcome ${name}!!. <a href="/list-todos">Click here</a> to view the list of Todo's.
</body>
</html>
```
Once you click on "Click Here"...you will get whitelabel error page since there is no list-todos available. So we will create the controller.

### Under controller create TodoController.java
```
package com.web.springbootwebapp.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.web.springbootwebapp.service.TodoService;


@Controller
public class TodoController {

	@Autowired
	TodoService service;
	
	@RequestMapping(value="/list-todos",method= RequestMethod.GET)	
	public String showTodos(ModelMap model){		
		model.put("todos",service.retrieveTodos("in28Minutes"));
		return "list-todos";
	}
	
	
	
}
```

### \src\main\webapp\WEB-INF\jsp\list-todos.jsp
```
<html>
<head>
<title>To Do's Application</title>
</head>
<body>
List of Todo's
${todos}
</body>
</html>

```

### Pending for Next Step
- ${name} is not available in list-todos.jsp
- in28Minutes is hardcoded in TodoController
- Login is not required to view the todolist(i.e Its not secure and concept of cookies not implemented)
- UI is not appealing and need more work

### Verify changes the starting the server

http://localhost:8080/list-todos




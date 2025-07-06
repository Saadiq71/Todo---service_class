# Todo---service_class

Absolutely! Here's a clean and beginner-friendly explanation you can directly use in your **GitHub README.md** file to describe the **Service layer** in your Spring Boot project:

---

## 📚 What is a Service Class in Spring Boot?

In Spring Boot, a **Service class** is used to **write the core business logic** of your application — that is, the real work like adding, updating, deleting, and fetching data.

Instead of writing all your logic directly in the `@RestController` (which handles HTTP requests), we **separate it out** into a **Service class** to keep the code clean and maintainable.

---

## ✅ Why use a Service Class?

| Without Service Class                        | With Service Class                      |
| -------------------------------------------- | --------------------------------------- |
| Controller handles everything (bad practice) | Controller only handles request routing |
| Hard to test/debug business logic            | Easy to test logic separately           |
| Not clean, not scalable                      | Clean separation of responsibilities    |

---

## 🛠️ How to Create and Use a Service Class

### 1️⃣ Create a Service Class

```java
package spring.application.todo.Todo.Application.Service;

import org.springframework.stereotype.Service;
import spring.application.todo.Todo.Application.Model.Todo;

import java.util.ArrayList;
import java.util.List;

@Service
public class TodoService {

    private List<Todo> todoList = new ArrayList<>();

    public String createTodo(int id, String title) {
        for (Todo todo : todoList) {
            if (todo.getId() == id) {
                return "Task already exists with this ID";
            }
        }
        todoList.add(new Todo(id, title, false));
        return "Todo added successfully";
    }

    public List<Todo> getAllTodos() {
        return todoList;
    }

    public String updateTitle(int id, String newTitle) {
        for (Todo todo : todoList) {
            if (todo.getId() == id) {
                todo.setTitle(newTitle);
                return "Title updated successfully";
            }
        }
        return "No todo found with this ID";
    }

    public String deleteTodo(int id) {
        for (int i = 0; i < todoList.size(); i++) {
            if (todoList.get(i).getId() == id) {
                todoList.remove(i);
                return "Todo deleted successfully";
            }
        }
        return "No todo found with this ID";
    }

    public String changeStatus(int id, boolean status) {
        for (Todo todo : todoList) {
            if (todo.getId() == id) {
                todo.setCompleted(status);
                return "Status updated successfully";
            }
        }
        return "No todo found with this ID";
    }
}
```

---

### 2️⃣ Use the Service in the Controller

```java
@RestController
@RequestMapping("/todo")
@CrossOrigin(origins = "*")
public class TodoController {

    @Autowired
    private TodoService todoService;

    @PostMapping("/create")
    public String create(@RequestParam int id, @RequestParam String title) {
        return todoService.createTodo(id, title);
    }

    @GetMapping("/all")
    public List<Todo> getAll() {
        return todoService.getAllTodos();
    }

    @PutMapping("/update")
    public String update(@RequestParam int id, @RequestParam String title) {
        return todoService.updateTitle(id, title);
    }

    @DeleteMapping("/delete")
    public String delete(@RequestParam int id) {
        return todoService.deleteTodo(id);
    }

    @PutMapping("/toggle-status")
    public String toggleStatus(@RequestParam int id, @RequestParam boolean status) {
        return todoService.changeStatus(id, status);
    }
}
```

---

## 🔍 Annotations Explained

### 🟢 `@Service`

* Added above your service class
* Tells Spring to treat this class as a **business logic layer**
* Spring automatically manages it like an object (bean)

### 🟢 `@Autowired`

* Used in your controller
* Tells Spring to **inject the object of the service class automatically**
* You don’t need to write `new TodoService()` manually

---

## 🧠 Summary

| Term          | Meaning                                            |
| ------------- | -------------------------------------------------- |
| `@Service`    | Marks the class as a service (where logic lives)   |
| `@Autowired`  | Injects the service object into controller         |
| Service class | Contains all functions: create, update, delete     |
| Controller    | Calls those service functions based on API request |

---

Would you like me to also generate this as a downloadable `README.md` file or include a code badge for Spring Boot?

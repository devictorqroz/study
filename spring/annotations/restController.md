
# Annotations

## @RestController

- @**RestController**: É uma conveniência que combina as anotações @**Controller** e @**ResponseBody**.


- **Função Principal:** Indica que a classe é um **controlador** Spring onde cada método **retorna um objeto de domínio** em vez de uma view. O objeto retornado é **serializado automaticamente para JSON ou XML** e enviado na **resposta** HTTP.


### Diferença entre @Controller e @RestController

- @**Controller**: Usado para classes que retornam **views**. **Necessário** usar @**ResponseBody** em métodos que retornam **dados** diretamente.


- @**RestController**: Usado para classes que **retornam dados diretamente (JSON/XML)**. @**ResponseBody** é implicitamente aplicado a todos os métodos.

---

### Exemplo de Uso

- Vamos criar um exemplo simples de um **controlador** ***RESTful*** usando @**RestController** em uma aplicação **Spring Boot**.


#### Estrutura do Projeto

**src / main / java / com / example**
 - ├── Application.java

 - ├── controller
 - -    └── UserController.java
 - └── model
 - - └── User.java


#### 1. Classe Principal (Application.java)

```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

#### 2. Controlador REST (UserController.java)

```java
package com.example.controller;

import com.example.model.User;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.List;

@RestController
public class UserController {

    private List<User> users = new ArrayList<>();

    @GetMapping("/users")
    public List<User> getAllUsers() {
        return users;
    }

    @GetMapping("/users/{id}")
    public User getUserById(@PathVariable Long id) {
        return users.stream().filter(user -> user.getId().equals(id)).findFirst().orElse(null);
    }

    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        users.add(user);
        return user;
    }
}
```

#### 3. Modelo (User.java)

```java
package com.example.model;

public class User {
    private Long id;
    private String name;

    // Construtores, getters e setters
    public User() {}

    public User(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

### Conclusão

- A annotation @**RestController** simplifica a criação de **controladores** _**RESTful**_ no Spring Boot. 


- Ela combina @**Controller** e @**ResponseBody**, tornando **desnecessário** adicionar @**ResponseBody** a cada método que retorna dados. Isso é especialmente útil para APIs que **retornam dados em formatos como JSON ou XML**.



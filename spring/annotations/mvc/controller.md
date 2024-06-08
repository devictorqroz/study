
# Annotations

## @Controller

- A annotation @**Controller** é uma das principais anotações do Spring MVC, usada para definir uma classe como um **controlador** Spring. 


- Esta classe será responsável por lidar com **solicitações** **HTTP** e retornar respostas apropriadas. 


- No contexto do **Spring** **Boot**, @**Controller** é frequentemente usada em combinação com outras anotações como @**RequestMapping**, @**GetMapping**, @**PostMapping**, entre outras, para **mapear** URLs para métodos específicos dentro do controlador.


### Definição e Propósito

- @**Controller**: Marca uma classe como um **controlador** Spring MVC. Indica ao Spring que essa classe contém **métodos** que devem ser tratados como **manipuladores** de **solicitações** **web**.


- **Função Principal:** **Separar** a lógica de **controle** da lógica de **apresentação**, atuando como **intermediário** entre o modelo de dados e as visualizações.

---

### Diferença entre @Controller e @RestController

- @**Controller**: Usado para **retornar** **views** (normalmente usando templates como Thymeleaf, JSP, etc.). **Não serializa automaticamente** os dados em JSON ou XML


- @**RestController**: **Combina** @**Controller** e @**ResponseBody**. Cada método nesta classe **retorna dados diretamente** em vez de uma view, e esses dados são **serializados** **automaticamente** para JSON ou XML.

---

### Exemplo de Uso

- Vamos criar um exemplo de um **controlador** simples usando @**Controller** em uma aplicação Spring Boot.

#### Estrutura do Projeto

src/main/java/com
- └── example
    - ├── Application.java
    - ├── controller
    - -    └── MyController.java
    - └── model
       - └── User.java


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

#### 2. Controlador (MyController.java)

```java
package com.example.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/users")
public class MyController {

    @GetMapping
    public String listUsers(Model model) {
        model.addAttribute("users", List.of(new User(1L, "John Doe"), new User(2L, "Jane Doe")));
        return "userList"; // Nome da view (template) a ser retornada
    }
}
```

#### 3. Modelo (User.java)

```java
package com.example.model;

public class User {
    private Long id;
    private String name;

    public User(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    // Getters e setters
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

#### 4. Template (Thymeleaf) userList.html

Crie um arquivo '**userList.html**' na pasta '**src/main/resources/templates**':

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>User List</title>
</head>
<body>
    <h1>List of Users</h1>
    <ul>
        <li th:each="user : ${users}">
            <span th:text="${user.id}"></span> - <span th:text="${user.name}"></span>
        </li>
    </ul>
</body>
</html>
```

---

### Conclusão

- A annotation @**Controller** é usada para definir uma classe como um **controlador** em uma aplicação **Spring MVC**. 


- Ela trabalha em **conjunto** com outras anotações para **mapear** **URLs** para **métodos** específicos dentro do controlador e **retorna** **views** que serão renderizadas.



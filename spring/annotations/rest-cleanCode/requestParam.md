
# Annotations

## @RequestParam

### Definição e Propósito

- @**RequestParam**: Annotation usada em controladores Spring MVC para extrair parâmetros de consulta (query parameters) da URL de uma requisição HTTP e vinculá-los aos parâmetros dos métodos manipuladores.


---

### Cenário Prático

#### Estrutura do Projeto

```
src
└── main
    └── java
        └── com
            └── example
                ├── Application.java
                ├── controller
                    └── UserController.java
                ├── service
                    └── UserService.java
                ├── repository
                    └── UserRepository.java
                └── model
                    ├── User.java
└── resources
    └── application.properties
```


#### Controlador (UserController.java)

Vamos criar um controlador que utiliza @**RequestParam** para extrair parâmetros de consulta das URLs.

```java
package com.example.controller;

import com.example.model.User;
import com.example.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return new ResponseEntity<>(createdUser, HttpStatus.CREATED);
    }

    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.getAllUsers();
        return new ResponseEntity<>(users, HttpStatus.OK);
    }

    @GetMapping("/email")
    public ResponseEntity<User> getUserByEmail(@RequestParam String email) {
        User user = userService.getUserByEmail(email);
        return new ResponseEntity<>(user, HttpStatus.OK);
    }

    @GetMapping("/name")
    public ResponseEntity<List<User>> getUsersByName(@RequestParam String name) {
        List<User> users = userService.getUsersByName(name);
        return new ResponseEntity<>(users, HttpStatus.OK);
    }

    @GetMapping("/search")
    public ResponseEntity<List<User>> getUsersByNameAndOptionalEmail(
            @RequestParam String name,
            @RequestParam(required = false) String email) {
        List<User> users;
        if (email != null) {
            users = List.of(userService.getUserByEmail(email));
        } else {
            users = userService.getUsersByName(name);
        }
        return new ResponseEntity<>(users, HttpStatus.OK);
    }
}
```


--- 


### Explicação do Código

#### Controlador (UserController.java)

- Define endpoints RESTful para criar usuários e buscar usuários por email ou nome usando **UserService**.


- **@RequestParam String email** extrai o parâmetro **email** da URL.


- **@RequestParam String name** extrai o parâmetro name da URL.


- **@RequestParam(required = false) String email** permite que o parâmetro **email** seja opcional.


---

### Benefícios de Usar @RequestParam

- **Legibilidade**: Facilita a leitura e compreensão dos métodos do controlador, pois os parâmetros são explícitos.


- **Flexibilidade**: Permite a manipulação de parâmetros de consulta de maneira fácil e eficiente.


- **Parâmetros Opcionais**: Permite definir parâmetros opcionais com **required = false**.


---


### Considerações Práticas

- **Validação**: Valide os parâmetros de consulta para garantir que os valores recebidos sejam válidos.


- **Valores Padrão**: Use **defaultValue** para fornecer valores padrão para parâmetros opcionais.


- **Documentação**: Documente claramente os parâmetros de consulta esperados para cada endpoint.


---


### Exemplos Avançados

#### Parâmetros com Valores Padrão

```java
@GetMapping("/search")
public ResponseEntity<List<User>> searchUsers(
        @RequestParam(defaultValue = "") String name,
        @RequestParam(defaultValue = "") String email) {
    List<User> users;
    if (!email.isEmpty()) {
        users = List.of(userService.getUserByEmail(email));
    } else {
        users = userService.getUsersByName(name);
    }
    return new ResponseEntity<>(users, HttpStatus.OK);
}
```


#### Parâmetros Múltiplos e Opcionais

```java
@GetMapping("/advanced-search")
public ResponseEntity<List<User>> advancedSearch(
        @RequestParam(required = false) String name,
        @RequestParam(required = false) String email) {
    List<User> users;
    if (name != null && email != null) {
        users = List.of(userService.getUserByNameAndEmail(name, email));
    } else if (name != null) {
        users = userService.getUsersByName(name);
    } else if (email != null) {
        users = List.of(userService.getUserByEmail(email));
    } else {
        users = userService.getAllUsers();
    }
    return new ResponseEntity<>(users, HttpStatus.OK);
}
```

---


### Conclusão

- A annotation @**RequestParam** é uma ferramenta poderosa no Spring MVC para manipular parâmetros de consulta nas requisições HTTP. Ela permite que os desenvolvedores criem endpoints RESTful que são flexíveis, fáceis de usar e alinhados com as boas práticas de desenvolvimento e arquitetura limpa.


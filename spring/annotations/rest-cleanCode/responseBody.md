
# Annotations

## @

### Definição e Propósito

- @**ResponseBody**: Annotation que indica que o valor de retorno de um método deve ser serializado diretamente no corpo da resposta HTTP.


- **Função Principal**: Permite que um método de controlador envie um valor de retorno que será automaticamente convertido em JSON, XML, ou outro formato adequado, e enviado como resposta HTTP.


---

### Uso Básico

- Utilizada em métodos que retornam dados diretamente em vez de exibir uma visão (view).

- Comumente usada em APIs RESTful para retornar objetos de domínio ou DTOs como JSON.


---

### Cenário Prático

Vamos considerar uma aplicação Spring Boot que segue boas práticas de arquitetura limpa e código limpo, usando Java 17. A aplicação gerencia uma entidade **User**.


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
                ├── model
                    └── User.java
                └── dto
                    └── UserRequest.java
└── resources
    └── application.properties
```

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

#### 2. Modelo (User.java)

```java
package com.example.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class User {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  private String name;
  private int age;

  // Getters e Setters

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

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }
}
```

#### 3. DTO (UserRequest.java)

```java
package com.example.dto;

import javax.validation.constraints.NotBlank;
import javax.validation.constraints.Positive;

public class UserRequest {
    
    @NotBlank
    private String name;

    @Positive
    private int age;

    // Getters e Setters

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

#### 4. Repositório (UserRepository.java)

```java
package com.example.repository;

import com.example.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

#### 5. Serviço (UserService.java)

```java
package com.example.service;

import com.example.dto.UserRequest;
import com.example.model.User;
import com.example.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User createUser(UserRequest userRequest) {
        User user = new User();
        user.setName(userRequest.getName());
        user.setAge(userRequest.getAge());
        return userRepository.save(user);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}
```

#### 6. Controlador (UserController.java)

```java
package com.example.controller;

import com.example.dto.UserRequest;
import com.example.model.User;
import com.example.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;
import java.util.List;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody UserRequest userRequest) {
        User user = userService.createUser(userRequest);
        return new ResponseEntity<>(user, HttpStatus.CREATED);
    }

    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.getAllUsers();
        return new ResponseEntity<>(users, HttpStatus.OK);
    }
}
```

#### 7. Configuração do Banco de Dados H2 (application.properties)

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

---


### Explicação do Código

#### Classe Principal (Application.java):

- Inicializa a aplicação Spring Boot usando **SpringApplication.run**.


#### Modelo (User.java):

- Representa a entidade **User** com campos básicos e seus respectivos getters e setters.

- Anotada com @**Entity**, @**Id**, e @**GeneratedValue** para mapear a entidade no banco de dados.


#### DTO (UserRequest.java):

- Data Transfer Object (DTO) para encapsular os dados da solicitação de criação de um usuário.

- Utiliza anotações de validação do Bean Validation API (@**NotBlank** e @**Positive**) para garantir a integridade dos dados.


#### Repositório (UserRepository.java):

- Interface que estende **JpaRepository** para fornecer operações de banco de dados CRUD para a entidade **User**.


#### Serviço (UserService.java):

- Contém a lógica de negócios para criar um usuário. Converte o DTO em uma entidade e realiza a operação de salvamento no banco de dados.

- Também inclui um método para recuperar todos os usuários do banco de dados.


#### Controlador (UserController.java):

* Exposto como um controlador REST com @**RestController**.

* Define endpoints POST (**/api/users**) e GET (**/api/users**) mapeados pelos **métodos** createUser e **getAllUsers**, respectivamente.

* Usa @**RequestBody** para vincular o corpo da solicitação JSON ao DTO **UserRequest** no método **createUser**.

* Retorna a lista de usuários com @**ResponseBody** implícito no método **getAllUsers**.


#### Configuração do Banco de Dados H2 (application.properties):

- Configura o banco de dados H2 em memória, com suporte a uma console H2 acessível via **/h2-console**.


---

### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método createUser com dados JSON
curl -X POST http://localhost:8080/api/users -H "Content-Type: application/json" -d '{"name":"John","age":30}'
# Resposta esperada: User created com ID e dados correspondentes

# Teste do método getAllUsers para listar todos os usuários
curl -X GET http://localhost:8080/api/users
# Resposta esperada: Lista de usuários em formato JSON
```

---

### Benefícios de Usar @ResponseBody

- **Serialização Automática**: Converte automaticamente o objeto Java retornado em JSON ou outro formato adequado.

- **Resposta Direta**: Facilita o envio de dados diretamente no corpo da resposta HTTP, essencial para APIs RESTful.

- **Eliminação de View**: Remove a necessidade de usar uma view resolver para renderizar a resposta.


---


### Considerações

- **Formato de Dados**: O Spring Boot suporta JSON por padrão (usando Jackson) e XML (usando JAXB) para serialização

- **Tratamento de Erros**: Se a serialização falhar, uma exceção será lançada, resultando em um código de resposta HTTP 500 (Internal Server Error).


### Conclusão

- A annotation @**ResponseBody** simplifica o processo de envio de dados como resposta HTTP, facilitando a criação de APIs RESTful robustas e seguindo boas práticas de arquitetura limpa e código limpo.


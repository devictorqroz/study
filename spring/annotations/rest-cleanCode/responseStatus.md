

# Annotations

## @ResponseStatus

### Definição e Propósito

- @**ResponseStatus**: Annotation usada para marcar um **método** ou uma **classe** com um código de **status HTTP** específico que deve ser retornado em resposta.

- **Função Principal**: Define o **código de status HTTP** (como 200 OK, 404 Not Found, 500 Internal Server Error) que será **retornado** quando um método de controlador for executado com sucesso.


---

### Uso Básico

- Utilizada em métodos de controladores para especificar explicitamente o código de status HTTP da resposta.


- Pode ser usada em exceções personalizadas para retornar códigos de status específicos quando essas exceções são lançadas.


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
                ├── model
                    └── User.java
                └── dto
                    └── UserRequest.java
                └── exception
                    └── UserNotFoundException.java
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
import com.example.exception.UserNotFoundException;
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

    public User getUserById(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new UserNotFoundException(id));
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}
```

#### 6. Exceção Personalizada (UserNotFoundException.java)

```java
package com.example.exception;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(HttpStatus.NOT_FOUND)
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(Long id) {
        super("User not found with id: " + id);
    }
}
```

#### 7. Controlador (UserController.java)

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
    @ResponseStatus(HttpStatus.CREATED)
    public User createUser(@Valid @RequestBody UserRequest userRequest) {
        return userService.createUser(userRequest);
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        User user = userService.getUserById(id);
        return new ResponseEntity<>(user, HttpStatus.OK);
    }

    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.getAllUsers();
        return new ResponseEntity<>(users, HttpStatus.OK);
    }
}
```

#### 8. Configuração do Banco de Dados H2 (application.properties)

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

- Data Transfer Object (**DTO**) para encapsular os dados da solicitação de criação de um usuário.

- Utiliza anotações de validação do Bean Validation API (@**NotBlank** e @**Positive**) para garantir a integridade dos dados.


#### Repositório (UserRepository.java):

- Interface que estende **JpaRepository** para fornecer operações de banco de dados CRUD para a entidade **User**.


#### Serviço (UserService.java):

- Contém a lógica de negócios para criar um usuário, recuperar um usuário por ID e listar todos os usuários.

- Lança uma exceção personalizada **UserNotFoundException** se um usuário não for encontrado pelo ID fornecido.


#### Exceção Personalizada (UserNotFoundException.java):

- Anotada com **@ResponseStatus(HttpStatus.NOT_FOUND)**.

- Lançada quando um usuário com o ID fornecido não é encontrado no banco de dados.


#### Controlador (UserController.java):

- Exposto como um controlador REST com @**RestController**.

- Define endpoints POST (/api/users), GET (/api/users), e GET com um parâmetro de caminho (/api/users/{id}).

- Usa @**RequestBody** para vincular o corpo da solicitação JSON ao DTO **UserRequest** no método **createUser**.

- Usa **@ResponseStatus(HttpStatus.CREATED)** no método **createUser** para definir o código de status HTTP 201 (Created).

- Retorna um **ResponseEntity** com o código de status HTTP adequado nos métodos **getUserById** e **getAllUsers**.


#### Configuração do Banco de Dados H2 (application.properties):

- Configura o banco de dados H2 em memória, com suporte a uma console H2 acessível via **/h2-console**.


---


### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método createUser com dados JSON
curl -X POST http://localhost:8080/api/users -H "Content-Type: application/json" -d '{"name":"John","age":30}'
# Resposta esperada: User created com ID e dados correspondentes (HTTP 201 Created)

# Teste do método getUserById para recuperar um usuário pelo ID
curl -X GET http://localhost:8080/api/users/1
# Resposta esperada: Dados do usuário com ID 1 (HTTP 200 OK)

# Teste do método getAllUsers para listar todos os usuários
curl -X GET http://localhost:8080/api/users
# Resposta esperada: Lista de usuários em formato JSON (HTTP 200 OK)

# Teste do método getUserById com um ID inexistente
curl -X GET http://localhost:8080/api/users/999
# Resposta esperada: Mensagem de erro "User not found with id: 999" (HTTP 404 Not Found)
```


---

### Benefícios de Usar @ResponseStatus

- **Controle Preciso do Código de Status**: Permite definir explicitamente o código de status HTTP retornado pelo método do controlador ou pela exceção personalizada.

- **Leitura e Manutenção Fáceis**: Facilita a leitura e manutenção do código ao tornar explícito qual código de status será retornado para determinadas ações ou erros.

- **Melhor Manipulação de Exceções**: Com exceções personalizadas anotadas com @ResponseStatus, o tratamento de erros torna-se mais consistente e centralizado.


---


### Considerações

- **Códigos de Status Customizados**: @**ResponseStatus** pode ser usado para retornar qualquer código de status HTTP definido no **HttpStatus**.


- **Exceções Personalizadas**: Utilizar @**ResponseStatus** em exceções personalizadas promove uma separação clara entre lógica de negócios e manipulação de erros.


---


### Conclusão

- A annotation @**ResponseStatus** oferece uma maneira poderosa e clara de definir o código de status HTTP que deve ser retornado por métodos de controlador ou exceções personalizadas, alinhando-se com as melhores práticas de arquitetura limpa e código limpo em aplicações Spring Boot.


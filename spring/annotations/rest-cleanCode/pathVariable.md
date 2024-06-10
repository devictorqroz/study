
# Annotations

## @PathVariable

### Definição e Propósito

- @**PathVariable**: Annotation usada para extrair valores de variáveis de caminho (**URL**) e vinculá-los a **parâmetros** de método no controlador.


- **Função Principal**: Facilita a captura de segmentos da URL e os mapeia diretamente para os parâmetros do método.

---

### Uso Básico

- Utilizada em métodos de controladores para obter valores dinâmicos da URL.


- Comumente usada em APIs RESTful para capturar IDs de recursos ou outras informações variáveis diretamente da URL.


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

    public void deleteUserById(Long id) {
        if (!userRepository.existsById(id)) {
            throw new UserNotFoundException(id);
        }
        userRepository.deleteById(id);
    }

    public User updateUser(Long id, UserRequest userRequest) {
        User user = userRepository.findById(id).orElseThrow(() -> new UserNotFoundException(id));
        user.setName(userRequest.getName());
        user.setAge(userRequest.getAge());
        return userRepository.save(user);
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

    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUserById(id);
    }

    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @Valid @RequestBody UserRequest userRequest) {
        User updatedUser = userService.updateUser(id, userRequest);
        return new ResponseEntity<>(updatedUser, HttpStatus.OK);
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


#### Modelo (User.java)
    
- Representa a entidade **User** com campos básicos e seus respectivos getters e setters.

- Anotada com @**Entity**, @**Id**, e @**GeneratedValue** para mapear a entidade no banco de dados.


#### DTO (UserRequest.java)
    
- Data Transfer Object (**DTO**) para encapsular os dados da solicitação de criação ou atualização de um usuário.

- Utiliza anotações de validação do Bean Validation API (@**NotBlank** e @**Positive**) para garantir a integridade dos dados.


#### Repositório (UserRepository.java)
    
- Interface que estende **JpaRepository** para fornecer operações de banco de dados CRUD para a entidade **User**.


#### Serviço (UserService.java)
    
- Contém a lógica de negócios para criar, recuperar, atualizar e deletar um usuário.

- Lança uma exceção personalizada **UserNotFoundException** se um usuário não for encontrado pelo ID fornecido.


#### Exceção Personalizada (UserNotFoundException.java)
    
- Anotada com **@ResponseStatus(HttpStatus.NOT_FOUND)**.

- Lançada quando um usuário com o ID fornecido não é encontrado no banco de dados.


#### Controlador (UserController.java)
    
- Exposto como um controlador REST com @**RestController**.

- Define endpoints POST (**/api/users**), GET (**/api/users** e **/api/users/{id}**), DELETE (**/api/users/{id}**) e PUT (**/api/users/{id}**). 

- Usa @**PathVariable** para vincular o valor do caminho da URL aos parâmetros dos métodos.

#### Exemplos de Uso de @PathVariable

1. **Recuperar um usuário pelo ID**:

- Método **getUserById** usa @**PathVariable** Long id para capturar o ID do usuário da URL.

```java
@GetMapping("/{id}")
public ResponseEntity<User> getUserById(@PathVariable Long id) {
    User user = userService.getUserById(id);
    return new ResponseEntity<>(user, HttpStatus.OK);
}
```

2. **Deletar um usuário pelo ID**:

- Método **deleteUser** usa @**PathVariable** Long id para capturar o ID do usuário da URL.

```java
@DeleteMapping("/{id}")
@ResponseStatus(HttpStatus.NO_CONTENT)
public void deleteUser(@PathVariable Long id) {
    userService.deleteUserById(id);
}
```

3. **Atualizar um usuário pelo ID**:

- Método **updateUser** usa @**PathVariable** Long id para capturar o ID do usuário da URL.

```java
@PutMapping("/{id}")
public ResponseEntity<User> updateUser(@PathVariable Long id, @Valid @RequestBody UserRequest userRequest) {
    User updatedUser = userService.updateUser(id, userRequest);
    return new ResponseEntity<>(updatedUser, HttpStatus.OK);
}
```

---


### Benefícios de Usar @PathVariable

- **URLs Descritivas e Amigáveis**: Facilita a criação de URLs descritivas, que são importantes para APIs RESTful.


- **Simplicidade e Clareza**: O uso de @**PathVariable** torna o código mais simples e claro, pois vincula diretamente segmentos da URL aos parâmetros do método.


- **Flexibilidade**: Permite a captura de múltiplas variáveis de caminho, o que é útil para operações que dependem de várias informações dinâmicas na URL.


---


### Considerações

- **Correspondência Exata dos Nomes**: O nome do parâmetro em @PathVariable deve corresponder ao nome na URL ou ser especificado explicitamente.


- **Validação e Tratamento de Erros**: Considere a validação e o tratamento de erros adequados quando trabalhar com valores de caminho para garantir a robustez da aplicação.


#### Correspondência Exata dos Nomes

**Exemplo**

Vamos considerar um controlador onde queremos capturar o **id** de um usuário a partir da URL. 
Se o nome do parâmetro @**PathVariable** não corresponder exatamente ao nome na URL, podemos especificar o nome explicitamente.

1. **Correspondência Exata dos Nomes**: Se o nome do parâmetro no método for exatamente o mesmo que na URL.

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUserById(@PathVariable Long id) {
    User user = userService.getUserById(id);
    return new ResponseEntity<>(user, HttpStatus.OK);
}
```


2. **Especificação Explícita do Nome**: Se o nome do parâmetro no método for diferente do nome na URL, podemos usar name no @PathVariable.

```java
@GetMapping("/users/{userId}")
public ResponseEntity<User> getUserById(@PathVariable(name = "userId") Long id) {
    User user = userService.getUserById(id);
    return new ResponseEntity<>(user, HttpStatus.OK);
}
```

---

### Conclusão

- A annotation @**PathVariable** é fundamental para a construção de APIs RESTful no Spring Boot, permitindo a extração e vinculação de valores dinâmicos da URL aos parâmetros do método do controlador. Isso facilita a criação de endpoints flexíveis e descritivos, alinhando-se com as melhores práticas de arquitetura limpa e código limpo.


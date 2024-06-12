
# Annotations

## @Query

### Definição e Propósito

- @**Query**: Annotation usada em repositórios Spring Data JPA para definir consultas SQL ou JPQL personalizadas diretamente nos métodos do repositório.


- **Função Principal**: Permite a criação de consultas complexas que não podem ser facilmente expressas usando os métodos de convenção do Spring Data JPA.


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


#### 2. Arquivo de Configuração (application.properties)

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
```

#### 3. Modelo (User.java)

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
    private String email;

    // Getters and Setters
}
```


#### 4. Repositório (UserRepository.java)

Vamos criar um repositório com métodos personalizados usando a annotation @**Query**

```java
package com.example.repository;

import com.example.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {

    @Query("SELECT u FROM User u WHERE u.email = :email")
    User findByEmail(@Param("email") String email);

    @Query("SELECT u FROM User u WHERE u.name LIKE %:name%")
    List<User> findByNameContains(@Param("name") String name);
}
```


#### 5. Serviço (UserService.java)

```java
package com.example.service;

import com.example.model.User;
import com.example.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User getUserByEmail(String email) {
        return userRepository.findByEmail(email);
    }

    public List<User> getUsersByName(String name) {
        return userRepository.findByNameContains(name);
    }
}
```


#### 6. Controlador (UserController.java)

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
}
```


---


### Explicação do Código

#### Repositório (UserRepository.java):

* Contém métodos personalizados usando @**Query** para buscar usuários por email e nome.


* **@Query("SELECT u FROM User u WHERE u.email = :email")** define uma consulta JPQL para buscar usuários por email.


* **@Query("SELECT u FROM User u WHERE u.name LIKE %:name%")** define uma consulta JPQL para buscar usuários cujo nome contém a string fornecida.


---


#### Benefícios de Usar @Query


- **Flexibilidade**: Permite a criação de consultas complexas que não podem ser facilmente criadas usando os métodos de convenção do Spring Data JPA.


- **Legibilidade**: As consultas são definidas diretamente nos métodos do repositório, tornando o código mais legível e fácil de manter


- **Parâmetros Nomeados**: Facilita a passagem de parâmetros para a consulta, aumentando a clareza e reduzindo a possibilidade de erros.


---


### Considerações

- **Performance**: Consultas personalizadas podem ser otimizadas para melhorar a performance, especialmente em bancos de dados grandes.


- **Manutenção**: Consultas complexas podem ser difíceis de manter, por isso é importante garantir que elas sejam bem documentadas e testadas.


- **Segurança**: Sempre use parâmetros nomeados (@Param) para evitar injeção de SQL.


---


### Exemplos Avançados

#### Consulta Nativa

```java
@Query(value = "SELECT * FROM users WHERE email = :email", nativeQuery = true)
User findByEmailNative(@Param("email") String email);
```


#### Consulta com Múltiplos Parâmetros

```java
@Query("SELECT u FROM User u WHERE u.name = :name AND u.email = :email")
User findByNameAndEmail(@Param("name") String name, @Param("email") String email);
```


#### Consulta com Ordenação

```java
@Query("SELECT u FROM User u WHERE u.name LIKE %:name% ORDER BY u.email ASC")
List<User> findByNameContainsOrderByEmail(@Param("name") String name);
```


---


#### Conclusão

- A annotation @**Query** é uma ferramenta poderosa no Spring Data JPA para definir consultas personalizadas de maneira declarativa e eficiente. Ela permite que os desenvolvedores criem consultas complexas e específicas diretamente nos repositórios, mantendo o código limpo e alinhado com as boas práticas de desenvolvimento.


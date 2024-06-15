
# Annotations

## @Param 

### Definição e Propósito

- @**Param**: Annotation usada em repositórios Spring Data JPA para definir parâmetros de consultas JPQL ou SQL nativas. Ela permite que você passe valores para os parâmetros nomeados dentro das consultas definidas pelo @**Query**.


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

Vamos criar um repositório com métodos personalizados usando @**Query** e @**Param**.


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

    @Query("SELECT u FROM User u WHERE u.name = :name AND u.email = :email")
    User findByNameAndEmail(@Param("name") String name, @Param("email") String email);
}
```


#### 5. Serviço (UserService.java)

```java
package com.example.service;

import com.example.model.User;
import com.example.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

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

    public User getUserByNameAndEmail(String name, String email) {
        return userRepository.findByNameAndEmail(name, email);
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

    @GetMapping("/search")
    public ResponseEntity<User> getUserByNameAndEmail(@RequestParam String name, @RequestParam String email) {
        User user = userService.getUserByNameAndEmail(name, email);
        return new ResponseEntity<>(user, HttpStatus.OK);
    }
}
```

---


### Explicação do Código

#### Classe Principal (Application.java):

-


#### Repositório (UserRepository.java):

- Contém métodos personalizados usando @**Query** para buscar usuários por email, nome e uma combinação de ambos.


- **@Param("email")** vincula o parâmetro do método **findByEmail** ao parâmetro nomeado :**email** na consulta JPQL.


    
- **@Param("email")** vincula o parâmetro do método **findByEmail** ao parâmetro nomeado :**email** na consulta JPQL.


- **@Param("name")** e **@Param("email")** vinculam os parâmetros do método **findByNameAndEmail** aos parâmetros nomeados :**name** e :**email** na consulta JPQL.


---


### Benefícios de Usar @Param

- **Legibilidade**: Facilita a leitura e compreensão das consultas, pois os parâmetros nomeados são explícitos.


- **Flexibilidade**: Permite a criação de consultas dinâmicas e complexas sem depender apenas da convenção de nomes de métodos do Spring Data JPA.


- **Segurança**: Ajuda a evitar injeção de SQL, pois os parâmetros são vinculados de forma segura.


---

### Considerações Práticas

- **Nomes Significativos**: Use nomes de parâmetros significativos para melhorar a legibilidade do código.


- **Consistência**: Mantenha consistência na nomeação dos parâmetros entre os métodos e as consultas para evitar confusão.


- **Testes**: Teste cuidadosamente as consultas personalizadas para garantir que elas se comportem conforme esperado.


---


### Exemplos Avançados

#### Consulta com Parâmetros Múltiplos

```java
@Query("SELECT u FROM User u WHERE u.name = :name AND u.email = :email")
User findByNameAndEmail(@Param("name") String name, @Param("email") String email);
```


#### Consulta com Parâmetros Opcionais

```java
@Query("SELECT u FROM User u WHERE (:name IS NULL OR u.name = :name) AND (:email IS NULL OR u.email = :email)")
List<User> findByNameOrEmail(@Param("name") String name, @Param("email") String email);
```


---


### Conclusão

- A annotation @**Param** é uma ferramenta essencial no Spring Data JPA para vincular parâmetros de métodos a parâmetros de consulta de forma segura e legível. Ela permite a criação de consultas personalizadas e dinâmicas que são fáceis de entender e manter, seguindo boas práticas de desenvolvimento e arquitetura limpa.


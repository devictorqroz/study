
# Annotations

## @RequestBody

Vamos explorar a annotation @**RequestBody**, que é usada no Spring MVC para **mapear** o **corpo** de uma **solicitação HTTP** para um **objeto** em Java.


### Definição e Propósito

- @**RequestBody**: É uma annotation que vincula o **corpo da solicitação HTTP** ao objeto de transferência de dados **(DTO)** ou a um **objeto de domínio** do método do controlador.


- **Função Principal**: **Deserializar** automaticamente o **corpo** da solicitação HTTP (em formatos como **JSON** ou **XML**) em um **objeto** Java, usando o mecanismo de **conversão HTTP** do Spring.


---

### Uso Básico

- Utilizada principalmente em **métodos** que lidam com **dados** recebidos via **solicitações HTTP** _POST, PUT e PATCH_.


- Facilita a criação de **APIs** **_RESTful_**, permitindo que **dados** estruturados sejam enviados e processados facilmente.


---

### Cenário Prático

Vamos considerar uma aplicação Spring Boot que segue boas práticas de arquitetura limpa e código limpo, usando Java 17. A aplicação gerencia uma entidade User.


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
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import javax.validation.Valid;

@RestController
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping("/users")
    public ResponseEntity<User> createUser(@Valid @RequestBody UserRequest userRequest) {
        User user = userService.createUser(userRequest);
        return new ResponseEntity<>(user, HttpStatus.CREATED);
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

- **Representa** a entidade **User** com campos básicos e seus respectivos getters e setters.


- Anotada com @**Entity**, @**Id**, e @**GeneratedValue** para mapear a entidade no banco de dados.


#### DTO (UserRequest.java):

- Data Transfer Object (**DTO**) para **encapsular os dados** da solicitação de criação de um **usuário**.


- Utiliza anotações de validação do **Bean Validation API** (@**NotBlank** e @**Positive**) para garantir a **integridade** dos dados.


#### Repositório (UserRepository.java):

- Interface que estende **JpaRepository** para fornecer operações de banco de dados _CRUD_ para a entidade **User**.


#### Serviço (UserService.java):

- Contém a **lógica de negócios** para criar um usuário. **Converte o DTO em uma entidade** e **realiza a operação** de salvamento no banco de dados.


#### Controlador (UserController.java):

- Exposto como um controlador **REST** com @**RestController**.
- Define um endpoint POST (**/users**) mapeado pelo método **createUser**.
- Usa @**RequestBody** para vincular o corpo da solicitação JSON ao DTO **UserRequest**.
- Usa @**Valid** para acionar a validação do DTO.
- **Retorna** uma resposta **HTTP 201 (Created)** com o **usuário** criado.


#### Configuração do Banco de Dados H2 (application.properties):

- Configura o banco de dados H2 em memória, com suporte a uma console H2 acessível via **/h2-console**.


---

### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método createUser com dados JSON
curl -X POST http://localhost:8080/users -H "Content-Type: application/json" -d '{"name":"John","age":30}'
# Resposta esperada: User created com ID e dados correspondentes
```

---

### Benefícios de Usar @RequestBody

- **Deserialização Automática**: Converte automaticamente o corpo da solicitação em um objeto Java.

- **Validação**: Integrada com @**Valid** para garantir que os dados recebidos atendam aos requisitos especificados.

- **Separa Responsabilidades:** Mantém a lógica de negócios e a lógica de controle **separadas**, seguindo o princípio da **responsabilidade única**.


---


### Considerações

- **Formato de Dados**: O Spring Boot suporta JSON por padrão (usando Jackson) e XML (usando JAXB) para deserialização.

- **Tratamento de Erros**: Se a deserialização falhar ou a validação não for bem-sucedida, uma exceção será lançada, resultando em um código de resposta HTTP 400 (Bad Request).


### Conclusão

- A annotation @**RequestBody** simplifica o processo de leitura e conversão do corpo da solicitação HTTP em objetos Java, facilitando a criação de APIs RESTful robustas e seguindo boas práticas de arquitetura limpa e código limpo.


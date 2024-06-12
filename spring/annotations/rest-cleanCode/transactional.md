
# Annotations

## @Transactional
#### # propagations

### Definição e Propósito

- @**Transactional**: Annotation usada para gerenciar transações declarativas no Spring.


- **Função Principal**: Facilita a gestão de transações, garantindo que um bloco de código (geralmente métodos de serviço) seja executado dentro de uma transação. Se uma exceção ocorrer, a transação pode ser revertida (rollback), garantindo a integridade dos dados.

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

    @Transactional
    public User createUser(User user) {
        return userRepository.save(user);
    }

    @Transactional(readOnly = true)
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    @Transactional
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
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

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return new ResponseEntity<>(HttpStatus.NO_CONTENT);
    }
}
```

---


### Explicação do Código

#### Serviço (UserService.java):

- Contém métodos transacionais para criar, listar e deletar usuários.

- @**Transactional** garante que essas operações são executadas dentro de uma transação.
- **@Transactional(readOnly = true)** otimiza operações de leitura, melhorando a performance.


### Benefícios de Usar @Transactional

- **Atomicidade**: Garante que todas as operações dentro de uma transação sejam completadas com sucesso ou todas falhem.


- **Rollback Automático**: Reverte automaticamente as mudanças no banco de dados em caso de exceção não verificada.


- **Simplificação do Código**: Elimina a necessidade de gerenciar manualmente as transações.


---


### Considerações

- **Exceções Verificadas** vs. **Não Verificadas**: Por padrão, o Spring só faz rollback em exceções não verificadas (**RuntimeException** e subclasses). Para exceções verificadas, é necessário configurá-las explicitamente.


- **Leitura** vs. **Escrita**: Use **@Transactional(readOnly = true)** para operações de leitura para **otimizar** o desempenho.


- **Propagação**: Entender os diferentes tipos de propagação de transações (**REQUIRED**, **REQUIRES_NEW**, etc.) é crucial para garantir o comportamento correto das transações.


---

### Propagação de Transações 

- A propagação de transações define como uma transação existente deve se comportar quando uma transação é **executada dentro de outra transação**.

- O Spring oferece várias opções de propagação que permitem um controle fino sobre o comportamento das transações aninhadas.

#### 1. REQUIRED (Padrão

- **Descrição**: Se uma transação já existe, o método será executado dentro dessa transação. Caso contrário, uma nova transação será iniciada.

- **Exemplo**: Método **A** chama método **B**. Se **A** já estiver em uma transação, **B** usará essa transação. Se **A** não estiver em uma transação, uma nova transação será criada para **B**.


```java
@Transactional(propagation = Propagation.REQUIRED)
public void methodA() {
    // Este método executará em uma nova transação ou na transação existente.
}

@Transactional(propagation = Propagation.REQUIRED)
public void methodB() {
    // Este método usará a mesma transação de methodA se for chamado dentro de methodA.
}
```

#### 2. REQUIRES_NEW

- **Descrição**: Sempre inicia uma nova transação. Se já houver uma transação existente, ela será suspensa durante a execução do método anotado com **REQUIRES_NEW**.


- **Exemplo**: Método **A** chama método **B**. **B** sempre terá sua própria transação, independente da transação de **A**.


```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void methodB() {
    // Este método sempre executará em uma nova transação.
}
```

#### 3. SUPPORTS

- **Descrição**: Se uma transação já existir, o método será executado dentro dessa transação. Caso contrário, ele será executado sem transação.


- **Exemplo**: Método **A** chama método **B**. **B** usará a transação de **A** se existir, caso contrário, executará sem transação.


```java
@Transactional(propagation = Propagation.SUPPORTS)
public void methodB() {
    // Este método usará a transação existente ou executará sem transação.
}
```

#### 4. NOT_SUPPORTED

- **Descrição**: Sempre executa o método fora de uma transação. Se uma transação já existir, ela será suspensa durante a execução do método.


- **Exemplo**: Método **A** chama método **B**. **B** sempre executará sem transação, independente da transação de **A**.


```java
@Transactional(propagation = Propagation.NOT_SUPPORTED)
public void methodB() {
    // Este método sempre executará fora de uma transação.
}
```

#### 5. MANDATORY

- **Descrição**: Deve ser executado dentro de uma transação existente. Se nenhuma transação existir, uma exceção será lançada.


- **Exemplo**: Método **A** chama método **B**. **B** só executará se **A** estiver em uma transação; caso contrário, uma exceção será lançada.


```java
@Transactional(propagation = Propagation.MANDATORY)
public void methodB() {
    // Este método exigirá uma transação existente.
}
```

#### 6. NEVER

- **Descrição**: Deve ser executado fora de uma transação. Se uma transação já existir, uma exceção será lançada.


- **Exemplo**: Método **A** chama método **B**. **B** só executará se **A** não estiver em uma transação; caso contrário, uma exceção será lançada.


```java
@Transactional(propagation = Propagation.NEVER)
public void methodB() {
    // Este método nunca deve ser executado dentro de uma transação.
}
```


#### 7. NESTED

- **Descrição**: Se uma transação já existir, o método será executado dentro de uma "sub-transação" (transação aninhada). Se a transação principal falhar, as mudanças feitas pela sub-transação serão revertidas. Se não houver transação, será criado como **REQUIRED**.


- **Exemplo**: Método **A** chama método **B**. **B** executará em uma transação aninhada se **A** estiver em uma transação; caso contrário, uma nova transação será criada.


```java
@Transactional(propagation = Propagation.NESTED)
public void methodB() {
    // Este método executará em uma sub-transação se houver uma transação existente.
}
```

---


#### Cenário Prático com Propagação

#### Estrutura do Projeto

* **Controlador** (TransactionController.java): Endpoint para iniciar a operação.
* **Serviço** (TransactionService.java): Métodos com diferentes tipos de propagação.
* **Repositório** (UserRepository.java): Repositório JPA para acesso ao banco de dados.


#### Implementação


**1. Classe Principal (Application.java)**


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

**2. Modelo (User.java)**

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

    // Getters and Setters
}
```

**3. Repositório (UserRepository.java)**


```java
package com.example.repository;

import com.example.model.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
}
```

4. Serviço (TransactionService.java)

```java
package com.example.service;

import com.example.model.User;
import com.example.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

@Service
public class TransactionService {

    @Autowired
    private UserRepository userRepository;

    @Transactional(propagation = Propagation.REQUIRED)
    public void requiredMethod() {
        // Código da transação principal
        User user = new User();
        user.setName("Required User");
        userRepository.save(user);

        nestedMethod();  // Chama o método aninhado
    }

    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void requiresNewMethod() {
        // Código que sempre executa em uma nova transação
        User user = new User();
        user.setName("Requires New User");
        userRepository.save(user);
    }

    @Transactional(propagation = Propagation.NESTED)
    public void nestedMethod() {
        // Código que executa em uma sub-transação
        User user = new User();
        user.setName("Nested User");
        userRepository.save(user);
    }
}
```

**5. Controlador (TransactionController.java)**


```java
package com.example.controller;

import com.example.service.TransactionService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/transactions")
public class TransactionController {

    @Autowired
    private TransactionService transactionService;

    @GetMapping("/required")
    public String executeRequired() {
        transactionService.requiredMethod();
        return "Executed REQUIRED propagation";
    }

    @GetMapping("/requires-new")
    public String executeRequiresNew() {
        transactionService.requiresNewMethod();
        return "Executed REQUIRES_NEW propagation";
    }

    @GetMapping("/nested")
    public String executeNested() {
        transactionService.nestedMethod();
        return "Executed NESTED propagation";
    }
}
```

---

### Considerações Sobre Propagações

#### Benefícios da Propagação de Transações

1. Controle Fino: Permite controle detalhado sobre como as transações devem se comportar em diferentes cenários.


2. Flexibilidade: Facilita a implementação de operações complexas que exigem diferentes comportamentos transacionais.


3. Segurança: Garante que operações críticas sejam concluídas com sucesso ou revertidas em caso de falha.


#### Considerações Práticas

1. **Desempenho**: Use propagação de transações de maneira adequada para evitar overhead desnecessário.


2. **Design**: Planeje o design do serviço e a interação entre métodos transacionais para evitar dependências complexas.


3. **Testes**: Teste cuidadosamente o comportamento transacional para garantir que o rollback e o commit funcionem como esperado em todos os cenários.


- A compreensão da propagação de transações e sua correta aplicação é essencial para desenvolver aplicativos robustos e confiáveis com Spring.


---

### Exemplos Avançados

#### Rollback em Exceções Verificadas:

- Configurando explicitamente o rollback para exceções verificadas.

```java
@Transactional(rollbackFor = Exception.class)
public void someMethod() throws Exception {
    // method implementation
}
```


#### Configuração de Propagação:

- Especificando o comportamento de propagação de transações.
  java

  
```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void someOtherMethod() {
    // method implementation
}
```

---


### Conclusão

- A annotation @**Transactional** é uma ferramenta essencial para a gestão de transações no Spring, garantindo integridade, atomicidade e simplificação do código. Ela permite a criação de métodos transacionais de forma declarativa, alinhando-se com as melhores práticas de arquitetura limpa e código limpo.


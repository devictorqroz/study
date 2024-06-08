
# Annotations 

## @Service

A annotation @**Service** é uma das estereotipadas do **Spring** Framework que são **usadas para definir classes que contêm a lógica de negócios.** 

Vamos explorar a annotation @**Service** em detalhes, incluindo seu uso, propósito e como ela se encaixa no ecossistema **Spring**.


### Definição Básica

A annotation @**Service** é aplicada a classes no Spring para indicar que são classes de **serviço**. 

Estas são geralmente classes que **contêm lógica de negócios** e chamam métodos de repositórios para realizar operações **CRUD** (Create, Read, Update, Delete).

```java
import org.springframework.stereotype.Service;

@Service
public class UserService {
    
    // Lógica de negócios aqui
}
```

### Propósito e Uso

- **Lógica de Negócios:** @**Service** é usada para marcar classes que contêm a lógica de negócios, **separando-a** das camadas de controle e persistência.


- **Componente de Spring:** @**Service** é uma **specialization** de @**Component**, o que significa que classes anotadas com @**Service** são detectadas automaticamente pelo Spring durante a varredura de componentes e registradas como **beans** no contexto da aplicação.


- **Claridade Semântica:** Usar @**Service** em vez de @**Component** aumenta a **clareza** do código, indicando explicitamente que a classe é responsável pela lógica de negócios.


---

### Exemplo de Uso

Vamos ver um exemplo prático de uma aplicação **Spring** onde @**Service** é usada junto com outras camadas:

#### Entidade

```java
import javax.persistence.*;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
    private String email;

    // Getters e Setters
}
```

#### Repositório

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}
```

#### Serviço

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public List<User> findAllUsers() {
        return userRepository.findAll();
    }

    public User findUserByUsername(String username) {
        return userRepository.findByUsername(username);
    }

    public User saveUser(User user) {
        return userRepository.save(user);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

#### Controlador

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAllUsers();
    }

    @GetMapping("/{username}")
    public User getUserByUsername(@PathVariable String username) {
        return userService.findUserByUsername(username);
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.saveUser(user);
    }

    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

---

### Detalhes Internos e Boas Práticas

- **Injeção de Dependências:** O uso de @**Service** permite que o Spring **injete** automaticamente dependências (como UserRepository) usando @**Autowired**.


- **Aspectos Transversais:** Serviços anotados com @**Service** podem ser alvo de aspectos **transversais**, como transações (@**Transactional**), segurança, e logging.


- **Testabilidade:** Classes de serviço são facilmente **testáveis** usando **mocks** para dependências como repositórios.

--- 

## Conclusão

- A annotation @**Service** é uma ferramenta poderosa no Spring Framework para estruturar e organizar a lógica de negócios de uma aplicação. 


- Ela oferece uma maneira clara e semântica de definir classes de serviço, facilita a injeção de dependências e permite a aplicação de aspectos transversais de maneira eficaz.


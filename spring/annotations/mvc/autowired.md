
# Annotations

## @Autowired

A annotation @**Autowired** é uma das ferramentas mais poderosas do Spring Framework para a **injeção de dependências**. 

Ela permite que o Spring resolva e **injete beans** automaticamente em seus **componentes**, simplificando a gestão de dependências e promovendo a **inversão de controle**.
 

### Definição Básica

A annotation @**Autowired** é aplicada a **campos**, **métodos de setter** ou **construtores** para indicar que o Spring deve injetar automaticamente uma dependência. 

O Spring resolve o bean apropriado e o injeta na classe onde a annotation está presente.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    // Métodos de serviço que utilizam userRepository
}
```

---

### Injeção em Diferentes Locais

#### Injeção em Campos

A maneira mais comum e direta de usar @**Autowired** é anotando um campo diretamente. 

No entanto, essa abordagem não é recomendada em termos de testabilidade e design orientado a objetos, pois viola o princípio da inversão de controle e torna mais difícil a injeção de mocks durante os testes.

```java
@Autowired
private UserRepository userRepository;
```

&nbsp;

#### Injeção em Construtores

A injeção de **construtores** é a maneira recomendada de usar @**Autowired**, pois promove a imutabilidade e facilita a escrita de testes unitários. 

Com a introdução do Spring 4.3, a annotation @**Autowired** em construtores não é mais necessária se houver apenas um construtor.

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // Métodos de serviço
}
```

&nbsp;

#### Injeção em Métodos Setter

A injeção de métodos setter é útil quando a dependência é **opcional** ou pode ser **alterada** após a construção do objeto.

```java
@Service
public class UserService {

    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // Métodos de serviço
}
```

---

### Modo Obrigatório ou Opcional

Por padrão, @**Autowired** espera que o bean a ser injetado exista.
Se o bean não for encontrado, uma exceção será lançada. 
No entanto, você pode tornar a injeção **opcional** usando o atributo **required**.

```java
@Autowired(required = false)
private UserRepository userRepository;
```

---

### Qualificadores

Quando há múltiplos **beans** do mesmo **tipo** no contexto Spring, pode ocorrer uma **ambiguidade**. 

Para resolver isso, você pode usar a annotation @**Qualifier** junto com @**Autowired** para **especificar** qual bean deve ser injetado.

```java
import org.springframework.beans.factory.annotation.Qualifier;

@Autowired
@Qualifier("specificUserRepository")
private UserRepository userRepository;
```

---

### Exemplos Práticos

#### Exemplo Completo com Injeção de Construtores

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Transactional(readOnly = true)
    public List<User> findAllUsers() {
        return userRepository.findAll();
    }

    @Transactional
    public User saveUser(User user) {
        return userRepository.save(user);
    }
}
```

---

### Ciclo de Vida de Beans e @Autowired

- **O Spring gerencia o ciclo de vida dos *beans*, e a injeção de dependências ocorre durante a *criação* dos beans, *antes* que o contexto de aplicação esteja totalmente pronto. O Spring verifica o contexto de aplicação, resolve as dependências necessárias e injeta os beans apropriados.**

---

## Conclusão

- A annotation @**Autowired** é uma ferramenta essencial no Spring Framework para a **injeção** de dependências, promovendo a **modularidade**, **testabilidade** e **manutenção** do código.


-  Ela facilita a gestão de dependências e integrações entre diferentes **componentes** da aplicação, garantindo que as dependências sejam resolvidas e injetadas **automaticamente** pelo Spring.


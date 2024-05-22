
# Annotations

## @entity

- parte do JPA (Java Persistence API)
- mapeamento de classes com tabelas do banco de dados.
- é usada para indicar que uma classe Java representa uma entidade no banco de dados. 

- A annotation @Entity é aplicada a uma classe para especificar que ela é uma entidade JPA. Aqui está um exemplo básico:


```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class User {

    @Id
    private Long id;
    private String name;
    private String email;

    // Getters and Setters
}
```
---

&nbsp;
### Atributos da Annotation @Entity
- name: Especifica o nome da entidade. 
- Se não for especificado, o nome da classe será usado como o nome da entidade.


```java
@Entity(name = "UserEntity")
public class User {
    // ...
}
```

---

&nbsp;
### Uso em Projetos Spring

- No contexto de um projeto Spring, especialmente com Spring Data JPA, a annotation @Entity é usada em conjunto com outras annotations JPA para definir o mapeamento da classe Java para a tabela do banco de dados e suas colunas.

---

&nbsp;
### Funcionalidade e Ciclo de Vida

- Quando uma classe é marcada como @Entity, o JPA e o provedor ORM (como Hibernate) cuidam de várias tarefas, incluindo:

  - Gerenciamento automático do ciclo de vida da entidade (persistência, atualização, remoção).
  - Mapeamento das propriedades da classe para as colunas da tabela.
  - Suporte a operações de banco de dados via EntityManager ou repositórios Spring Data JPA.

---

&nbsp;
### Integração com Repositórios Spring Data JPA 

- Para facilitar o acesso e manipulação de dados, geralmente usamos repositórios Spring Data JPA. Veja como isso se integra com a entidade '**User**':


```java

import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}

```

---


## Conclusão

###### A annotation @Entity é essencial para trabalhar com JPA e Spring Data JPA, fornecendo uma maneira clara e declarativa de mapear classes Java para tabelas de banco de dados. 

###### Combinada com outras annotations e a infraestrutura de repositórios do Spring, ela permite uma abordagem poderosa e eficiente para a persistência de dados.

&nbsp;














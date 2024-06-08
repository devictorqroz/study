
# Annotations 

## @Id  
### @GeneratedValue
- **(AUTO, IDENTITY, SEQUENCE, TABLE)**
### @SequenceGenerator
### @TableGenerator

&nbsp;

- A annotation **@Id** é uma das mais fundamentais no JPA, pois é usada para marcar o campo de uma entidade que será a chave primária. 

- Vamos explorar a annotation **@Id** em detalhes, incluindo seu uso e como ela interage com outras annotations para definir o comportamento da chave primária.

&nbsp;

### Detalhes sobre @Id

#### Definição Básica

- A annotation **@Id** é aplicada a um campo ou propriedade de uma entidade JPA para indicar que ele é a chave primária da tabela correspondente no banco de dados.

Aqui está um exemplo básico:

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

### Geração de Chaves Primárias

- Na maioria dos casos, não queremos definir manualmente os valores das chaves primárias. Em vez disso, usamos mecanismos de geração automática de chaves. Para isso, combinamos **@Id** com **@GeneratedValue**.

#### @GeneratedValue

- A annotation @**GeneratedValue** é usada para especificar a estratégia de geração de valores para a chave primária. Ela possui vários atributos que permitem configurar a geração de chaves:



- **strategy**: Define a estratégia de geração de chave. Pode ser uma das seguintes:
  - **GenerationType.AUTO:** A estratégia padrão, que deixa o provedor de persistência decidir a melhor estratégia com base no banco de dados.
  
  - **GenerationType.IDENTITY:** Usa uma coluna auto-incrementada do banco de dados para gerar os valores das chaves.

  - **GenerationType.SEQUENCE:** Usa uma sequência de banco de dados para gerar os valores das chaves.

  - **GenerationType.TABLE:** Usa uma tabela especial no banco de dados para gerar os valores das chaves.


- **generator**: Define o nome de um gerador de chave primária personalizado.

---

### Exemplo com @GeneratedValue

- Vamos ver um exemplo de uso de @**Id** com @**GeneratedValue**:

```java
import javax.persistence.*;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String password;
    private String email;

    // Getters e setters
}
```

- Neste exemplo, a chave primária **id** será gerada automaticamente pelo banco de dados usando uma coluna auto-incrementada.

---

### Exemplos com Outras Estratégias de Geração

#### GenerationType.SEQUENCE

- Se quisermos usar uma sequência do banco de dados, definimos a estratégia como **GenerationType.SEQUENCE** e podemos usar a annotation @**SequenceGenerator** para definir a sequência:

```java
import javax.persistence.*;

@Entity
@SequenceGenerator(name = "user_seq", sequenceName = "user_sequence", allocationSize = 1)
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq")
    private Long id;

    private String username;
    private String password;
    private String email;

    // Getters e setters
}
```

&nbsp;

#### GenerationType.TABLE

- Para usar uma tabela específica para gerar chaves, usamos a estratégia **GenerationType.TABLE:**

```java
import javax.persistence.*;

@Entity
@TableGenerator(name = "user_gen", table = "id_gen", pkColumnName = "gen_name", valueColumnName = "gen_value", allocationSize = 1)
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.TABLE, generator = "user_gen")
    private Long id;

    private String username;
    private String password;
    private String email;

    // Getters e setters
}
```

---

## Conclusão

- A annotation @**Id** é crucial no JPA para identificar a chave primária de uma entidade. 


-  Quando combinada com @**GeneratedValue**, ela permite a geração automática de chaves primárias, simplificando muito o gerenciamento de entidades.


- A escolha da estratégia de geração de chaves depende dos requisitos específicos do seu projeto e das capacidades do banco de dados subjacente.




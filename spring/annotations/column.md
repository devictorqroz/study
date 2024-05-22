
# Annotations

## @Column

- A annotation @Column é usada no JPA (Java Persistence API) para definir detalhes sobre o mapeamento de uma propriedade ou campo de uma entidade para uma coluna na tabela do banco de dados. Ela fornece um controle fino sobre como os atributos da entidade são persistidos e recuperados.


### Detalhes sobre @Column

#### Definição Básica

- A annotation @**Column** é aplicada a um campo ou propriedade de uma entidade para especificar detalhes sobre o mapeamento da coluna correspondente no banco de dados. 

Aqui está um exemplo básico:

```java
import javax.persistence.*;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "username", nullable = false, unique = true)
    private String username;

    @Column(name = "password", nullable = false)
    private String password;

    @Column(name = "email", nullable = false, unique = true)
    private String email;

    // Getters e Setters
}
```

### Atributos da Annotation @Column

1. **name:** Especifica o nome da coluna no banco de dados. Se não for especificado, o nome do campo ou propriedade será usado.

    ```java
        @Column(name = "user_name")
        private String username;
    ```
   

2. **nullable**: Indica se a coluna pode conter valores nulos. O padrão é true.

    ```java
        @Column(nullable = false)
        private String password;
    ```

   
3. **unique**: Indica se a coluna deve ter valores únicos. O padrão é false.

    ```java
        @Column(unique = true)
        private String email;
    ```
  
 
4. **length**: Especifica o comprimento da coluna para campos de tipo String. O padrão é 255.

    ```java
       @Column(length = 50)
        private String username;
    ```
  
 
5. **precision**: Define a precisão (número total de dígitos) para colunas numéricas. Utilizado com **BigDecimal**.

    ```java
        @Column(precision = 10)
        private BigDecimal balance;
    ```


6. **scale**: Define a escala (número de dígitos à direita do ponto decimal) para colunas numéricas. Utilizado com **BigDecimal**.

    ```java
        @Column(precision = 10, scale = 2)
        private BigDecimal balance;
    ```


7. **insertable**: Indica se a coluna deve ser incluída em instruções SQL de inserção. O padrão é **true**.

    ```java
        @Column(insertable = false)
        private String createdBy;
    ```


8. **updatable**: Indica se a coluna deve ser incluída em instruções SQL de atualização. O padrão é **true**.

    ```java
        @Column(updatable = false)
        private String createdBy; 
    ```


9. **columnDefinition**: Permite definir a SQL literal para a coluna. Útil para especificar tipos de dados específicos do banco de dados.

    ```java
        @Column(columnDefinition = "TEXT")
        private String description;
    ```


10. **table**: Especifica a tabela secundária à qual esta coluna pertence se a entidade mapeia várias tabelas.

    ```java
        @Column(table = "secondary_table")
        private String secondaryField;
    ```


---

### Exemplo Completo

Vamos considerar um exemplo completo usando vários desses atributos:

```java
import javax.persistence.*;
import java.math.BigDecimal;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "user_name", nullable = false, unique = true, length = 50)
    private String username;

    @Column(name = "password", nullable = false)
    private String password;

    @Column(name = "email", nullable = false, unique = true)
    private String email;

    @Column(precision = 10, scale = 2)
    private BigDecimal balance;

    @Column(insertable = false, updatable = false)
    private String createdBy;

    @Column(columnDefinition = "TEXT")
    private String description;

    // Getters e Setters
}
```

---

## Conclusão

A annotation @**Column** oferece um controle detalhado sobre como os atributos de uma entidade são mapeados para as colunas no banco de dados, permitindo que você configure aspectos como nomes de colunas, unicidade, possibilidade de nulidade, comprimento, precisão e escala, entre outros. 

Isso é essencial para garantir que os dados sejam armazenados e recuperados de maneira consistente e eficiente.








# Annotations

## @Embeddable e @EmbeddedId

### Definição e Propósito

- @**Embeddable**: Annotation usada em JPA para indicar que uma classe pode ser **incorporada** em outras entidades. Isso permite a composição de objetos, onde o estado de uma entidade é incorporado em outra, simplificando o mapeamento de propriedades comuns.


- @**EmbeddedId**: Annotation usada em JPA para marcar uma propriedade de entidade como **chave primária composta**. A classe que representa a chave composta deve ser anotada com @**Embeddable**.


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
                    └── OrderController.java
                ├── service
                    └── OrderService.java
                ├── repository
                    └── OrderRepository.java
                └── model
                    ├── Order.java
                    └── OrderId.java
└── resources
    └── application.properties
```


#### Chave Composta (OrderId.java)

```java
package com.example.model;

import javax.persistence.Embeddable;
import java.io.Serializable;
import java.util.Objects;

@Embeddable
public class OrderId implements Serializable {

    private Long orderId;
    private Long productId;

    public OrderId() {}

    public OrderId(Long orderId, Long productId) {
        this.orderId = orderId;
        this.productId = productId;
    }

    // Getters and Setters

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        OrderId orderId1 = (OrderId) o;
        return Objects.equals(orderId, orderId1.orderId) &&
               Objects.equals(productId, orderId1.productId);
    }

    @Override
    public int hashCode() {
        return Objects.hash(orderId, productId);
    }
}
```


#### Entidade (Order.java)

```java
package com.example.model;

import javax.persistence.EmbeddedId;
import javax.persistence.Entity;

@Entity
public class Order {

    @EmbeddedId
    private OrderId id;

    private Integer quantity;
    private Double price;

    public Order() {}

    public Order(OrderId id, Integer quantity, Double price) {
        this.id = id;
        this.quantity = quantity;
        this.price = price;
    }

    // Getters and Setters
}
```


#### Repositório (OrderRepository.java)

```java
package com.example.repository;

import com.example.model.Order;
import com.example.model.OrderId;
import org.springframework.data.jpa.repository.JpaRepository;

public interface OrderRepository extends JpaRepository<Order, OrderId> {
}
```


#### Serviço (OrderService.java)

```java
package com.example.service;

import com.example.model.Order;
import com.example.repository.OrderRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class OrderService {

    @Autowired
    private OrderRepository orderRepository;

    public Order createOrder(Order order) {
        return orderRepository.save(order);
    }

    public List<Order> getAllOrders() {
        return orderRepository.findAll();
    }
}
```


---


### Explicação do Código

#### Chave Composta (OrderId.java):

- Marcada com @**Embeddable**, indicando que pode ser **incorporada** como parte da chave primária de uma entidade.


- Implementa **Serializable** e sobrescreve **equals** e **hashCode** para garantir a igualdade correta de instâncias.


#### Entidade (Order.java):

- Define a entidade **Order** com uma chave primária composta usando @**EmbeddedId**.


- Incorpora a classe **OrderId** como chave primária.


---

### Benefícios de Usar @Embeddable e @EmbeddedId

- **Reutilização de Código**: Facilita a reutilização de classes comuns em várias entidades.


- **Legibilidade**: Melhora a legibilidade e manutenção do código, encapsulando atributos relacionados em uma classe separada.


- **Simplicidade**: Simplifica o mapeamento JPA, evitando a criação de tabelas separadas para atributos frequentemente usados.


- **Chave Composta**: Permite a definição de chaves primárias compostas de forma limpa e explícita.


---


### Considerações Práticas

- **Encapsulamento**: Use @**Embeddable** para encapsular grupos de atributos relacionados que são usados por várias entidades.


- **Consistência**: Mantenha consistência ao usar @**Embeddable** para representar conceitos comuns em diferentes entidades.


- **Validação**: Considere adicionar validações aos campos da classe @**Embeddable**.


- **Chave Composta**: Garanta que a classe de chave composta implementa **Serializable** e sobrescreve **equals** e **hashCode**.


---


### Exemplos Avançados

#### Usando @AttributeOverrides e @AttributeOverride

Se você tiver atributos incorporados em uma entidade que precisam ser mapeados para colunas com nomes diferentes:

```java
@Entity
public class Company {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name = "street", column = @Column(name = "hq_street")),
        @AttributeOverride(name = "city", column = @Column(name = "hq_city")),
        @AttributeOverride(name = "state", column = @Column(name = "hq_state")),
        @AttributeOverride(name = "zipCode", column = @Column(name = "hq_zipCode"))
    })
    private Address headquartersAddress;

    @Embedded
    @AttributeOverrides({
        @AttributeOverride(name = "street", column = @Column(name = "billing_street")),
        @AttributeOverride(name = "city", column = @Column(name = "billing_city")),
        @AttributeOverride(name = "state", column = @Column(name = "billing_state")),
        @AttributeOverride(name = "zipCode", column = @Column(name = "billing_zipCode"))
    })
    private Address billingAddress;

    // Getters and Setters
}
```


---


### Conclusão

As annotations @**Embeddable** e @**EmbeddedId** são ferramentas poderosas no JPA para modelar **objetos compostos** e **chaves primárias compostas**, promovendo a reutilização e encapsulamento de atributos comuns. Elas simplificam o design e a manutenção de entidades, alinhando-se com as boas práticas de arquitetura limpa e código limpo.


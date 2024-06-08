
# Annotations

## @PutMapping

Vamos explorar a annotation @**PutMapping**, que é usada no Spring MVC para **mapear** solicitações **HTTP PUT** para métodos manipuladores.


### Definição e Propósito

- @**PutMapping**: É uma annotation específica para **mapear** solicitações **HTTP PUT** para métodos manipuladores.


- **Função Principal**: Fornecer uma maneira mais concisa e legível de **mapear** solicitações **_PUT_**, em comparação com @**RequestMapping**


---

### Uso Básico

- Pode ser usada para especificar o **caminho da URL** e, opcionalmente, outros **atributos** como _params, headers, produces, e consumes_.


---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**PutMapping** em uma aplicação Spring Boot.


#### Estrutura do Projeto

```
src
└── main
    └── java
        └── com
            └── example
                ├── Application.java
                └── controller
                    └── MyController.java
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


#### 2. Controlador (MyController.java)

```java
package com.example.controller;

import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/api")
public class MyController {

    // Exemplo simples de um método PUT
    @PutMapping("/update/{id}")
    @ResponseBody
    public String updateById(@PathVariable("id") Long id, @RequestBody User user) {
        // Lógica para atualizar o recurso pelo ID (por exemplo, atualizar no banco de dados)
        return "Updated resource with ID: " + id + " with new name: " + user.getName();
    }
}

// Classe User para deserializar JSON
class User {
    private String name;
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

---


### Explicação do Código

#### Classe Principal (Application.java):

- Inicializa a aplicação Spring Boot usando **SpringApplication.run**.


#### Controlador (MyController.java):

- Anotada com @**Controller** e **@RequestMapping("/api")**, indicando que t**odos os métodos** dentro dessa classe serão **mapeados** com o prefixo **_/api_**.


- **Método** _updateById_:

    - Anotado com **@PutMapping("/update/{id}")**, mapeando o caminho _**/api/update/{id}**_ para solicitações **_PUT_**.

    - Usa **@PathVariable("id")** para capturar o **valor {id} da URL** e vinculá-lo ao **parâmetro id** do método.

    - Usa @**RequestBody** para vincular o **corpo** da solicitação **JSON** ao objeto **_User_**.

    - **Retorna** uma mensagem indicando que o **recurso com o ID** fornecido foi **atualizado** com o novo nome.

  
---

### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método updateById com um ID específico e dados JSON
curl -X PUT http://localhost:8080/api/update/123 -H "Content-Type: application/json" -d '{"name":"John","age":30}'
# Resposta esperada: "Updated resource with ID: 123 with new name: John"
```

---

### Atributos Principais de @PutMapping

* **value (ou path)**: Define o(s) caminho(s) da **URL**.
* **params**: Especifica **parâmetros** de solicitação que precisam estar presentes.
* **headers**: Especifica **cabeçalhos** de solicitação que precisam estar presentes.
* **produces**: Especifica o tipo de mídia que o método pode **produzir**.
* **consumes**: Especifica o tipo de mídia que o método pode **consumir**.


---


### Conclusão

- A annotation @**PutMapping** simplifica a definição de **métodos** manipuladores de solicitações **_PUT_**, tornando o código mais conciso e legível em comparação com @**RequestMapping**.


- Ela é amplamente utilizada em **controladores** Spring MVC para criar **endpoints** **_RESTful_** que manipulam a **atualização** de recursos.


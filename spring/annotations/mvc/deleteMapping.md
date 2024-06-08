
# Annotations

## @DeleteMapping

Vamos explorar a annotation @**DeleteMapping**, que é usada no Spring MVC para **mapear** solicitações **HTTP DELETE** para métodos manipuladores.


### Definição e Propósito

- @**DeleteMapping**: É uma annotation específica para **mapear** solicitações **HTTP DELETE** para métodos manipuladores.


- **Função Principal**: Fornecer uma maneira mais concisa e legível de **mapear** solicitações **DELETE**, em comparação com @**RequestMapping**.


---

### Uso Básico

- Pode ser usada para **especificar** o caminho da **URL** e, opcionalmente, outros **atributos** como _params, headers, produces, e consumes._


---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**DeleteMapping** em uma aplicação Spring Boot.


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

import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/api")
public class MyController {

    // Exemplo simples de um método DELETE
    @DeleteMapping("/delete/{id}")
    @ResponseBody
    public String deleteById(@PathVariable("id") Long id) {
        // Lógica para deletar o recurso pelo ID (por exemplo, deletar do banco de dados)
        return "Deleted resource with ID: " + id;
    }
}
```

---


### Explicação do Código

#### Classe Principal (Application.java):

- Inicializa a aplicação Spring Boot usando **SpringApplication.run**.


#### Controlador (MyController.java):

- Anotada com @**Controller** e **@RequestMapping("/api")**, indicando que **todos os métodos** dentro dessa classe serão **mapeados** com o prefixo **/api**.


- **Método** _deleteById_:

    - Anotado com **@DeleteMapping("/delete/{id}")**, mapeando o caminho **_/api/delete/{id}_** para solicitações **_DELETE_**.

    - Usa **@PathVariable("id")** para capturar o **valor {id} da URL** e vinculá-lo ao **parâmetro id** do método.

    - **Retorna** uma mensagem indicando que o recurso com o **ID** fornecido foi **deletado**.

  
---

### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método deleteById com um ID específico
curl -X DELETE http://localhost:8080/api/delete/123
# Resposta esperada: "Deleted resource with ID: 123"
```

---

### Atributos Principais de @DeleteMapping

* **value (ou path)**: Define o(s) caminho(s) da **URL**.
* **params**: Especifica **parâmetros** de solicitação que precisam estar presentes.
* **headers**: Especifica **cabeçalhos** de solicitação que precisam estar presentes.
* **produces**: Especifica o tipo de mídia que o método pode **produzir**.
* **consumes**: Especifica o tipo de mídia que o método pode **consumir**.


---


### Conclusão

- A annotation @**DeleteMapping** simplifica a definição de **métodos** manipuladores de solicitações **DELETE**, tornando o código mais conciso e legível em comparação com @**RequestMapping**


- Ela é amplamente utilizada em **controladores** Spring MVC para criar **endpoints** **_RESTful_** que manipulam a **exclusão** de recursos.


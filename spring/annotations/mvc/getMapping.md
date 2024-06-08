
# Annotations

## @GetMapping

Vamos explorar a annotation @**GetMapping**, que é uma das **especializações** mais comuns de @**RequestMapping** no Spring MVC.


### Definição e Propósito

- @**GetMapping**: É uma annotation **específica** para **mapear** solicitações **HTTP GET** para métodos manipuladores.


- **Função Principal**: Fornecer uma maneira mais concisa e legível de **mapear** solicitações **GET**, em comparação com @**RequestMapping**.


---

### Uso Básico

- Pode ser usada para **especificar o caminho da URL** e, opcionalmente, outros **atributos** como _params, headers, produces, e consumes._


---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**GetMapping** em uma aplicação Spring Boot.


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

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.stereotype.Controller;

@Controller
@RequestMapping("/api")
public class MyController {

  // Parâmetro obrigatório
  @GetMapping("/greet")
  @ResponseBody
  public String greet(@RequestParam(name = "name") String name) {
    return "Hello, " + name + "!";
  }

  // Parâmetro opcional com valor padrão
  @GetMapping("/welcome")
  @ResponseBody
  public String welcome(@RequestParam(name = "name", defaultValue = "Guest") String name) {
    return "Welcome, " + name + "!";
  }

  // Parâmetro opcional sem valor padrão
  @GetMapping("/info")
  @ResponseBody
  public String info(@RequestParam(name = "name", required = false) String name) {
    if (name == null) {
      return "No name provided.";
    }
    return "Info for " + name;
  }

  // Vários valores para o mesmo parâmetro
  @GetMapping("/items")
  @ResponseBody
  public String items(@RequestParam(name = "item") List<String> items) {
    return "Items: " + String.join(", ", items);
  }
}
```

---


### Explicação do Código

#### Classe Principal (Application.java):

- Inicializa a aplicação Spring Boot usando **SpringApplication.run**.


#### Controlador (MyController.java):

- Anotada com @**Controller** e **@RequestMapping("/api")**, indicando que **todos os métodos dentro dessa classe** serão mapeados com o prefixo **/api**


- **Método** _greet_:

  - Anotado com **@GetMapping("/greet")**, mapeando o caminho _**/api/greet**_ para solicitações **_GET_**.

  - Usa **@RequestParam(name = "name")** para tornar o parâmetro _name_ **obrigatório**.

  - **Retorna** uma saudação personalizada.



- **Método** _welcome_:

  - Anotado com **@GetMapping("/welcome")**, mapeando o caminho _**/api/welcome**_ para solicitações **_GET_**.

  - Usa **@RequestParam(name = "name", defaultValue = "Guest")** para tornar o parâmetro name **opcional** com valor **padrão** _"Guest"_.

  - **Retorna** uma mensagem de boas-vindas personalizada.



- **Método** _info_:

  - Anotado com **@GetMapping("/info")**, mapeando o caminho **_/api/info_** para solicitações _**GET**_.

  - Usa **@RequestParam(name = "name", required = false)** para tornar o parâmetro name **opcional** **sem** valor padrão.

  - **Retorna** uma mensagem informativa baseada na presença do parâmetro **name**.


- **Método** _items_:

  - Anotado com **@GetMapping("/items")**, mapeando o caminho **_/api/items_** para solicitações **_GET_**.

  - Usa **@RequestParam(name = "item")** para capturar **múltiplos** valores do parâmetro item e retorná-los como uma **lista**.

  - **Retorna** uma lista de itens.

  
---

### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método greet com o parâmetro name
curl -X GET "http://localhost:8080/api/greet?name=John"
# Resposta esperada: "Hello, John!"

# Teste do método welcome com o parâmetro name
curl -X GET "http://localhost:8080/api/welcome?name=Jane"
# Resposta esperada: "Welcome, Jane!"

# Teste do método welcome sem o parâmetro name
curl -X GET "http://localhost:8080/api/welcome"
# Resposta esperada: "Welcome, Guest!"

# Teste do método info com o parâmetro name
curl -X GET "http://localhost:8080/api/info?name=John"
# Resposta esperada: "Info for John"

# Teste do método info sem o parâmetro name
curl -X GET "http://localhost:8080/api/info"
# Resposta esperada: "No name provided."

# Teste do método items com múltiplos valores para o parâmetro item
curl -X GET "http://localhost:8080/api/items?item=Apple&item=Banana&item=Cherry"
# Resposta esperada: "Items: Apple, Banana, Cherry"
```

---

### Atributos Principais de @GetMapping

 - **value (ou path)**: Define o(s) caminho(s) da URL.
 - **params**: Especifica parâmetros de solicitação que **precisam** estar presentes.
 - **headers**: Especifica cabeçalhos de solicitação que **precisam** estar presentes.
 - **produces**: Especifica o tipo de mídia que o método pode **produzir**.
 - **consumes**: Especifica o tipo de mídia que o método pode **consumir**.


---


### Conclusão

- A annotation @**GetMapping** simplifica a definição de métodos manipuladores de solicitações **GET**, tornando o código mais conciso e legível em comparação com @**RequestMapping**.

- Ela é amplamente utilizada em **controladores** Spring MVC para criar **endpoints** **_RESTful_**.



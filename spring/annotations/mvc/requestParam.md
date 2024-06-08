
# Annotations

## @RequestParam

### Definição e Propósito

- @**RequestParam**: É usada para **vincular** um parâmetro de _consulta_, parâmetro de _formulário_ ou _valor de um caminho_ de uma **solicitação HTTP** a um **parâmetro de método** no seu **controlador**.


- **Função Principal**: Facilitar a **extração de valores de parâmetros** de _consulta_ ou _formulários_ **HTTP** e vinculá-los aos parâmetros do **método** do controlador

---

### Uso Básico

-  Pode ser usado para especificar um **valor padrão** para o parâmetro.


- Pode marcar o parâmetro como **obrigatório** ou **opcional**.


- Pode ser usado para vincular um **único valor** ou uma **lista** de valores.


---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**RequestParam** em uma aplicação Spring Boot.


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

import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.stereotype.Controller;

@Controller
@RequestMapping("/api")
public class MyController {

  // Parâmetro obrigatório
  @RequestMapping(value = "/greet", method = RequestMethod.GET)
  @ResponseBody
  public String greet(@RequestParam(name = "name") String name) {
    return "Hello, " + name + "!";
  }

  // Parâmetro opcional com valor padrão
  @RequestMapping(value = "/welcome", method = RequestMethod.GET)
  @ResponseBody
  public String welcome(@RequestParam(name = "name", defaultValue = "Guest") String name) {
    return "Welcome, " + name + "!";
  }

  // Parâmetro opcional sem valor padrão
  @RequestMapping(value = "/info", method = RequestMethod.GET)
  @ResponseBody
  public String info(@RequestParam(name = "name", required = false) String name) {
    if (name == null) {
      return "No name provided.";
    }
    return "Info for " + name;
  }

  // Vários valores para o mesmo parâmetro
  @RequestMapping(value = "/items", method = RequestMethod.GET)
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

- Anotada com @**Controller** e **@RequestMapping("/api")**, indicando que todos os **métodos** dentro dessa classe serão **mapeados** com o prefixo _**/api**_.


- **Método** _greet_:

  - Anotado com **@RequestMapping(value = "/greet", method = RequestMethod.GET)**, mapeando o caminho **_/api/greet_** para solicitações **_GET_**.

  - Usa **@RequestParam(name = "name")** para tornar o parâmetro _**name**_ **obrigatório**.

  - **Retorna** uma saudação personalizada.

    
- **Método** _welcome_:

  - Anotado com **@RequestMapping(value = "/welcome", method = RequestMethod.GET)**, mapeando o caminho **_/api/welcome_** para solicitações **_GET_**.

  - Usa **@RequestParam(name = "name", defaultValue = "Guest")** para tornar o parâmetro **_name_** **opcional** com valor **padrão** _"Guest"_.

  - **Retorna** uma mensagem de boas-vindas personalizada.


- **Método** _info_:

  - Anotado com **@RequestMapping(value = "/info", method = RequestMethod.GET)**, mapeando o caminho _**/api/info**_ para solicitações **_GET_**.

  - Usa **@RequestParam(name = "name", required = false)** para tornar o parâmetro name **opcional** sem valor padrão.

  - **Retorna** uma mensagem informativa baseada na presença do parâmetro **name**.


- **Método** _items_:

  - Anotado com **@RequestMapping(value = "/items", method = RequestMethod.GET)**, mapeando o caminho **_/api/items_** para solicitações **_GET_**.

  - Usa **@RequestParam(name = "item")** para capturar **múltiplos** valores do parâmetro **item** e retorná-los como uma **lista**.

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

### Propriedades Principais de @RequestParam

 - **name** ou **value**: Define o **nome** do parâmetro da solicitação que deve ser vinculado ao parâmetro do método.


 - **required**: Indica se o parâmetro é **obrigatório**. O valor padrão é **true**


 - **defaultValue**: Especifica um valor **padrão** se o parâmetro **não** estiver presente na solicitação.


---


### Conclusão

- A annotation @**RequestParam** é uma ferramenta poderosa no Spring MVC para **extrair e vincular** parâmetros de solicitações **HTTP** aos **métodos dos controladores**.


 -  Ela oferece uma maneira simples e flexível de lidar com parâmetros **obrigatórios** e **opcionais**, bem como **múltiplos** valores.



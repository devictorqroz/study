
# Annotations

## @PostMapping

Vamos explorar a annotation @**PostMapping**, que é uma das **especializações** de @**RequestMapping** no Spring MVC, usada especificamente para **mapear** solicitações **HTTP POST**.


### Definição e Propósito

- @**PostMapping**: É uma annotation específica para **mapear** solicitações **HTTP POST** para métodos manipuladores.


- **Função Principal**: Fornecer uma maneira mais concisa e legível de **mapear** solicitações **POST**, em comparação com @**RequestMapping**

---

### Uso Básico

- Pode ser usada para **especificar o caminho da URL** e, opcionalmente, outros **atributos** como _params, headers, produces, e consumes_.


---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**PostMapping** em uma aplicação Spring Boot.


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

import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/api")
public class MyController {

    // Exemplo simples de um método POST
    @PostMapping("/submit")
    @ResponseBody
    public String submitData(@RequestBody String data) {
        return "Data submitted: " + data;
    }

    // POST com dados JSON
    @PostMapping("/user")
    @ResponseBody
    public String createUser(@RequestBody User user) {
        return "User created: " + user.getName();
    }

    // POST com dados de formulário
    @PostMapping("/form")
    @ResponseBody
    public String submitForm(@RequestParam(name = "name") String name, @RequestParam(name = "age") int age) {
        return "Form submitted: Name = " + name + ", Age = " + age;
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

- Anotada com @**Controller** e **@RequestMapping("/api")**, indicando que **todos os métodos** dentro dessa classe serão **mapeados** com o prefixo **/api**.


- **Método** _submitData_:

    - Anotado com **@PostMapping("/submit")**, mapeando o caminho _**/api/submit**_ para solicitações **_POST_**.

    - Usa @**RequestBody** para vincular o corpo da solicitação ao parâmetro **data**

    - **Retorna** a string recebida no **corpo** da solicitação


- **Método** _createUser_:

  - Anotado com **@PostMapping("/user")**, mapeando o caminho **_/api/user_** para solicitações _**POST**_.

  - Usa @**RequestBody** para vincular o **corpo** da solicitação **JSON** ao objeto _**User**_.

  - **Retorna** uma mensagem com o nome do usuário criado.


- **Método** _submitForm_:

  - Anotado com **@PostMapping("/form")**, mapeando o caminho **_/api/form**_ para solicitações **_POST_**.

  - Usa @**RequestParam** para vincular os parâmetros do formulário **name** e **age**.

  - **Retorna** uma mensagem com os dados do formulário submetidos.

  
---

### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método submitData com dados simples
curl -X POST http://localhost:8080/api/submit -H "Content-Type: text/plain" -d "Sample data"
# Resposta esperada: "Data submitted: Sample data"

# Teste do método createUser com dados JSON
curl -X POST http://localhost:8080/api/user -H "Content-Type: application/json" -d '{"name":"John","age":30}'
# Resposta esperada: "User created: John"

# Teste do método submitForm com dados de formulário
curl -X POST http://localhost:8080/api/form -d "name=Jane&age=25"
# Resposta esperada: "Form submitted: Name = Jane, Age = 25"
```

---

### Atributos Principais de @PostMapping

* **value (ou path)**: Define o(s) caminho(s) da **URL**.
* **params**: Especifica **parâmetros** de solicitação que precisam estar presentes.
* **headers**: Especifica **cabeçalhos** de solicitação que precisam estar presentes.
* **produces**: Especifica o tipo de mídia que o método pode **produzir**.
* **consumes**: Especifica o tipo de mídia que o método pode **consumir**.


---


### Conclusão

- A annotation @**PostMapping** simplifica a definição de métodos manipuladores de solicitações **POST**, tornando o código mais conciso e legível em comparação com @**RequestMapping**


-  Ela é amplamente utilizada em **controladores** Spring MVC para criar **endpoints** **_RESTful_** que manipulam dados enviados via **_POST_**.


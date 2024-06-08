
# Annotations

## @RequestMapping

### Definição e Propósito

- @**RequestMapping**: É usada para **mapear URLs** a métodos específicos de classes de **controladores**. Pode ser aplicada tanto a **classes** quanto a **métodos**.


- **Função Principal**: Definir como as **solicitações HTTP** são roteadas para os manipuladores apropriados.

---

### Uso Básico

- Pode ser usada para especificar o caminho da **URL** e o **método HTTP** (_GET, POST, PUT, DELETE_, etc.).


- Suporta diversos **atributos**, como _value (ou path), method, params, headers, produces, e consumes._


---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**RequestMapping** em uma aplicação Spring Boot.


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

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.stereotype.Controller;

@Controller
@RequestMapping("/myapp")
public class MyController {

    // Mapeamento para "/myapp/hello"
    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    @ResponseBody
    public String sayHello() {
        return "Hello, World!";
    }

    // Mapeamento para "/myapp/greet" com um parâmetro de consulta
    @RequestMapping(value = "/greet", method = RequestMethod.GET)
    @ResponseBody
    public String greet(@RequestParam(name = "name", defaultValue = "User") String name) {
        return "Hello, " + name + "!";
    }

    // Mapeamento para "/myapp/postdata" usando POST
    @RequestMapping(value = "/postdata", method = RequestMethod.POST)
    @ResponseBody
    public String postData() {
        return "Data posted successfully!";
    }
}
```

---


### Explicação do Código

#### Classe Principal (Application.java):

- Inicializa a aplicação Spring Boot usando **SpringApplication.run**.


#### Controlador (MyController.java):

- Anotada com @**Controller** e **@RequestMapping("/myapp")**, indicando que todos os métodos dentro dessa classe serão **mapeados** com o prefixo _**/myapp**_.


- **Método** _sayHello_:

  - Anotado com **@RequestMapping(value = "/hello", method = RequestMethod.GET)**, mapeando o caminho /**myapp/hello** para solicitações _**GET**_.

  - **Retorna** uma mensagem simples.

    
- **Método** _greet_:

  - Anotado com **@RequestMapping(value = "/greet", method = RequestMethod.GET)**, mapeando o caminho **/myapp/greet** para solicitações **_GET_**.

  - Usa @**RequestParam** para aceitar um **parâmetro** de consulta chamado **name**, com um valor padrão de "**_User_**".

  - **Retorna** uma saudação personalizada.


- **Método** _postData_:

  - Anotado com **@RequestMapping(value = "/postdata", method = RequestMethod.POST)**, mapeando o caminho **/myapp/postdata** para solicitações _**POST**_.

  - **Retorna** uma mensagem indicando que os dados foram **postados** com sucesso.


---

### Testando a Aplicação

1. Inicie a aplicação.


2. Acesse os endpoints:

```shell
# Teste do método sayHello
curl -X GET http://localhost:8080/myapp/hello
# Resposta esperada: "Hello, World!"

# Teste do método greet com o parâmetro name
curl -X GET "http://localhost:8080/myapp/greet?name=John"
# Resposta esperada: "Hello, John!"

# Teste do método postData
curl -X POST http://localhost:8080/myapp/postdata
# Resposta esperada: "Data posted successfully!"
```

---

### Atributos Principais de @RequestMapping

 - **value (ou path)**: Define o(s) caminho(s) da **URL**.


 - **method**: Especifica os **métodos HTTP** suportados (_GET, POST_, etc.).


 - **params**: Especifica **parâmetros** de solicitação que precisam estar **presentes**.


 - **headers**: Especifica **cabeçalhos** de solicitação que precisam estar **presentes**.


 - **produces**: Especifica o **tipo de mídia** que o método pode **produzir**.


 - **consumes**: Especifica o **tipo de mídia** que o método pode **consumir**.


---


### Exemplo Avançado

Vamos adicionar mais métodos ao controlador para demonstrar o uso de **params** e **headers**.

```java
@RequestMapping(value = "/filter", method = RequestMethod.GET, params = "type=advanced")
@ResponseBody
public String filterAdvanced() {
    return "Advanced filter applied!";
}

@RequestMapping(value = "/filter", method = RequestMethod.GET, params = "type=basic")
@ResponseBody
public String filterBasic() {
    return "Basic filter applied!";
}

@RequestMapping(value = "/headerCheck", method = RequestMethod.GET, headers = "X-Requested-With=XMLHttpRequest")
@ResponseBody
public String checkHeader() {
    return "Header is present!";
}
```

---


### Testando os Novos Métodos

#### Testar o filtro avançado:

```shell
curl -X GET "http://localhost:8080/myapp/filter?type=advanced"
# Resposta esperada: "Advanced filter applied!"
```

#### Testar o filtro básico:

```shell
curl -X GET "http://localhost:8080/myapp/filter?type=basic"
# Resposta esperada: "Basic filter applied!"
```

#### Testar a verificação de cabeçalho:

```shell
curl -X GET http://localhost:8080/myapp/headerCheck -H "X-Requested-With: XMLHttpRequest"
# Resposta esperada: "Header is present!"
```

---


### Conclusão

- A annotation @**RequestMapping** é uma ferramenta poderosa no Spring MVC para **mapear URLs a métodos específicos em controladores,** permitindo a criação de APIs web de maneira estruturada e flexível.

 -  Com seus diversos **atributos**, ela oferece uma grande flexibilidade para definir como as **solicitações HTTP** devem ser tratadas.


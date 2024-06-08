
# Annotations

## @Component

### Definição e Propósito

@**Component**: É uma annotation genérica usada para marcar uma classe como um **bean** gerenciado pelo Spring. Um bean é um **objeto** que é instanciado, montado e gerenciado pelo container do Spring.

**Função Principal**: Indicar ao Spring que uma classe deve ser registrada como um **bean** no contexto da aplicação.

---

### Diferentes Stereotypes de @Component

No Spring, além de @**Component**, existem outras anotações mais específicas que são derivadas de @Component, cada uma indicando um propósito específico:

- @**Service**: Indica que a classe provê algum **serviço**. É uma **especialização** de @Component.


- @**Repository**: Indica que a classe é um **repositório** de dados. Também é uma **especialização** de @Component.


- @**Controller**: Indica que a classe é um **controlador** Spring MVC. É uma **especialização** de @Component.


---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**Component** em uma aplicação Spring Boot.

#### Estrutura do Projeto


src.main.java.com.example
src.main.java.com.example.Application.java
src.main.java.com.example.component
src.main.java.com.example.component.MyComponent.java
src.main.java.com.example.service
src.main.java.com.example.service.MyService.java


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

#### 2. Componente (MyComponent.java)

```java
package com.example.component;

import org.springframework.stereotype.Component;

@Component
public class MyComponent {

    public String getMessage() {
        return "Hello from MyComponent!";
    }
}
```

#### 3. Serviço (MyService.java)

```java
package com.example.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.example.component.MyComponent;

@Service
public class MyService {

    private final MyComponent myComponent;

    @Autowired
    public MyService(MyComponent myComponent) {
        this.myComponent = myComponent;
    }

    public String deliverMessage() {
        return myComponent.getMessage();
    }
}
```

---

### Explicação do Código

#### Classe Principal (Application.java):

- Inicializa a aplicação Spring Boot usando **SpringApplication.run**.


#### Componente (MyComponent.java):

- Anotada com @**Component**, indica ao Spring que essa classe deve ser **gerenciada** como um **bean**.


- Contém um método **getMessage** que retorna uma mensagem.


#### Serviço (MyService.java):

- Anotada com @**Service**, indica que esta classe oferece algum tipo de serviço e deve ser gerenciada como um **bean**.


- Usa @**Autowired** no construtor para **injetar** _MyComponent_.

---

### Verificação

Vamos adicionar um **controlador** para expor o serviço via uma API REST e verificar se tudo está funcionando corretamente.

#### 4. Controlador (MyController.java)

```java
package com.example.controller;

import com.example.service.MyService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    private final MyService myService;

    @Autowired
    public MyController(MyService myService) {
        this.myService = myService;
    }

    @GetMapping("/message")
    public String getMessage() {
        return myService.deliverMessage();
    }
}
```

---

### Testando a Aplicação

1. **Inicie a aplicação.**
2. **Acesse o endpoint:**

`curl -X GET http://localhost:8080/message`


Você deverá ver a resposta "**Hello from MyComponent!**".

---

### Conclusão

A annotation @**Component** é uma das principais anotações do Spring, usada para indicar que uma **classe** deve ser **gerenciada** como um **bean** pelo container do Spring. 


Ela é amplamente utilizada para criar componentes **customizados** que podem ser facilmente **injetados** em outras partes da aplicação. 


@**Component** serve como base para outras anotações específicas como @**Service**, @**Repository** e @**Controller**.


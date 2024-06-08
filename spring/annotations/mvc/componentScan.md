
# Annotations

## @ComponentScan

### Definição e Propósito

- @**ComponentScan**: Configura a varredura de pacotes para **encontrar** classes anotadas com @**Component** e suas especializações (@**Service**, @**Repository**, @**Controller**).


- **Função Principal**: Permitir que o Spring **descubra** e **registre** beans automaticamente no contexto da aplicação, com base nas anotações.

---

### Local de Declaração

- @**ComponentScan** é geralmente usado em uma classe de **configuração** anotada com @**Configuration** ou em uma classe **principal** de uma aplicação Spring Boot anotada com @**SpringBootApplication**.

---

### Propriedades Principais

- **basePackages**: Define os pacotes a serem escaneados.

- **basePackageClasses**: Define as classes cujos pacotes devem ser escaneados.

---

### Exemplo de Uso

- Vamos criar um exemplo para entender como usar @**ComponentScan** em uma aplicação Spring Boot.


#### Estrutura do Projeto

```
src
└── main
    └── java
        └── com
            └── example
                ├── Application.java
                ├── config
                │   └── AppConfig.java
                ├── service
                │   └── MyService.java
                └── component
                    └── MyComponent.java
```


#### 1. Classe Principal (Application.java)

Vamos definir a classe principal da aplicação com @**ComponentScan**.

```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = "com.example")
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
    public String sayHello() {
        return "Hello from MyComponent";
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

    public String getGreeting() {
        return myComponent.sayHello();
    }
}
```

#### 4. Configuração Alternativa (AppConfig.java)

Opcionalmente, @**ComponentScan** pode ser colocado em uma **classe** de configuração separada.

```java
package com.example.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
    // Configurações adicionais podem ser adicionadas aqui
}
```

---

### Testando a Aplicação

Vamos adicionar um controlador para expor o serviço via uma API REST e verificar se tudo está funcionando corretamente.

#### Controlador (MyController.java)

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
        return myService.getGreeting();
    }
}
```

#### Testando a Aplicação

- Inicie a aplicação.


- Acesse o endpoint:

`curl -X GET http://localhost:8080/message`

#### Você deverá ver a resposta "Hello from MyComponent!".

---

### Conclusão

@**ComponentScan** é uma annotation poderosa no Spring que permite a **varredura** de pacotes para **encontrar** e **registrar** beans automaticamente.

Ela é essencial para a **autodetecção** de componentes e facilita a configuração e a organização de projetos Spring de forma modular e escalável.


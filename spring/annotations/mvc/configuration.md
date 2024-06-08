
# Annotation

## @Configuration

### Definição e Propósito

@**Configuration**: Marca uma **classe** como uma **fonte** de definições de **beans**. É uma maneira de usar **código Java para configurar beans**, em vez de usar arquivos XML.

**Função Principal**: Indicar ao Spring que esta **classe** pode ser usada pelo container de Spring **IoC (Inversion of Control)** como uma **fonte de definições de beans**.

---

### Diferença entre @Configuration e @Component

@**Configuration**: É uma **especialização** de @**Component**, usada especificamente para classes que **definem** beans. Isso significa que uma classe anotada com @**Configuration** é um **Spring bean** e c**ontém métodos que definem outros beans**.


@**Component**: É uma anotação **genérica** para marcar uma classe como um **bean** gerenciado pelo Spring.

---

### Exemplo de Uso

Vamos criar um exemplo para entender como usar @**Configuration** em uma aplicação Spring Boot.

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
                └── repository
                    └── MyRepository.java
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

#### 2. Configuração (AppConfig.java)

```java
package com.example.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.example.service.MyService;
import com.example.repository.MyRepository;

@Configuration
public class AppConfig {

    @Bean
    public MyRepository myRepository() {
        return new MyRepository();
    }

    @Bean
    public MyService myService() {
        return new MyService(myRepository());
    }
}
```

#### 3. Repositório (MyRepository.java)

```java
package com.example.repository;

public class MyRepository {

    public String fetchData() {
        return "Data from repository";
    }
}
```

#### 4. Serviço (MyService.java)

```java
package com.example.service;

import com.example.repository.MyRepository;

public class MyService {

    private final MyRepository myRepository;

    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }

    public String serve() {
        return "Service: " + myRepository.fetchData();
    }
}
```

---

### Conclusão

A annotation @**Configuration** é usada para marcar classes como **fontes de definições de beans** no Spring.

É uma maneira programática e tipada de definir e configurar beans, em contraste com a configuração baseada em XML. 

@**Configuration** trabalha em conjunto com @**Bean** para fornecer uma maneira robusta de **configurar** o contexto da aplicação Spring.



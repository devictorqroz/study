
# Annotations

## @Component x @Bean

@**Component** e @**Bean** são duas annotations usadas no Spring para definir **beans**, mas são usadas em contextos diferentes e têm diferenças importantes. 
Vamos explorar essas diferenças em detalhe.


### @Component

#### Definição e Uso

- @**Component**: **Marca** uma **classe** como um **bean** gerenciado pelo Spring.


- **Onde é Usado**: Diretamente na classe.


- **Uso Típico**: Para autodetecção de componentes (scanning).


#### Exemplo

```java
package com.example;

import org.springframework.stereotype.Component;

@Component
public class MyComponent {
    public String sayHello() {
        return "Hello from MyComponent";
    }
}
```

---

### @Bean

#### Definição e Uso

- @**Bean**: Define um **método** que **retorna** um **bean** a ser gerenciado pelo Spring.


- **Onde é Usado**: Em **métodos** dentro de uma **classe** anotada com @**Configuration**.


- **Uso Típico**: Para configuração **programática** de **beans**, especialmente quando você precisa configurar a instância de um bean de forma mais **detalhada** ou quando você precisa criar beans de bibliotecas de **terceiros** que você não pode anotar com @**Component**.


---

#### Exemplo

```java
package com.example;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public MyComponent myComponent() {
        return new MyComponent();
    }
}

public class MyComponent {
    public String sayHello() {
        return "Hello from MyComponent";
    }
}
```

---

### Diferenças Principais

#### 1. Declaração:

- @**Component**: Colocada diretamente na **classe**.


- @**Bean**: Colocada em um **método** dentro de uma classe de **configuração**.


#### 2. Escopo de Aplicação:

- @**Component**: Usado para autodetecção de componentes através de **scanning** de pacotes.


- @**Bean**: Usado para configuração **explícita** e detalhada de beans, muitas vezes quando você precisa configurar um bean de uma biblioteca de **terceiros**.


#### 3. Autodetecção:

- @**Component**: Requer que o Spring **escaneie** pacotes para descobrir beans anotados.


- @**Bean**: Não depende de autodetecção, os beans são **registrados** explicitamente no contexto do Spring.


#### 4. Flexibilidade:

- @**Component**: **Menos** flexível para configurações **complexas**. Geralmente usado para classes **simples** onde a injeção de dependência padrão é suficiente.


- @**Bean**: **Mais** flexível para configurações **complexas** e para instanciar objetos com lógica de criação **personalizada**.


---


### Quando Usar Qual

#### Use @Component quando:

- Você **tem** controle sobre a classe (você pode **modificar** o código-fonte).


- A configuração do bean é **simples** e não requer lógica de construção **complexa**.


- Você quer aproveitar o **scanning** de componentes para registrar seus beans.


#### Use @Bean quando:

- Você **não** tem controle sobre a classe (ex: classes de bibliotecas de **terceiros**).


- A criação do bean **requer** alguma lógica de construção **personalizada**.


- Você precisa configurar beans de forma mais **detalhada** ou **condicional**.


---


### Exemplo Combinado

Vamos ver um exemplo onde usamos ambos, @**Component** e @**Bean**.


#### Estrutura do Projeto

```
src
└── main
    └── java
        └── com
            └── example
                ├── Application.java
                ├── component
                │   └── MyComponent.java
                ├── config
                │   └── AppConfig.java
                └── service
                    └── MyService.java
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

#### 3. Configuração (AppConfig.java)

```java
package com.example.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public MyComponent myComponent() {
        return new MyComponent();
    }
}
```

#### 4. Serviço (MyService.java)

```java
package com.example.service;

import com.example.component.MyComponent;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

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

---

### Conclusão

- @**Component** e @**Bean** são duas maneiras de **definir** **beans** no Spring, mas são usadas em contextos **diferentes**. 


- @**Component** é ideal para autodetecção de classes simples


- @**Bean** é usado para configuração explícita e mais detalhada de beans.


**Ambas** as abordagens podem ser **combinadas** em uma aplicação Spring para tirar proveito de suas respectivas **vantagens**.


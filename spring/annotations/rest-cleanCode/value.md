
# Annotations

## @Value

### Definição e Propósito

- @**Value**: Annotation usada no Spring para injetar valores de propriedades externas em campos, métodos ou parâmetros do construtor.


- **Função Principal**: Facilita a configuração de valores de propriedades definidos em arquivos de configuração, como **application.properties, application.yml**, ou variáveis de ambiente.


---

### Uso Básico

- Pode ser usada para injetar strings, números, listas, mapas, e até mesmo valores complexos.

- Suporta expressões Spring Expression Language (SpEL).


---

### Cenário Prático

#### Estrutura do Projeto

```
src
└── main
    └── java
        └── com
            └── example
                ├── Application.java
                ├── controller
                    └── ConfigController.java
                ├── service
                    └── ConfigService.java
                └── model
                    └── ConfigProperties.java
└── resources
    └── application.properties
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


#### 2. Arquivo de Configuração (application.properties)

```properties
app.name=MyApplication
app.version=1.0.0
app.description=This is a sample Spring Boot application
```

#### 3. Modelo (ConfigProperties.java)

```java
package com.example.model;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class ConfigProperties {

    @Value("${app.name}")
    private String appName;

    @Value("${app.version}")
    private String appVersion;

    @Value("${app.description}")
    private String appDescription;

    // Getters

    public String getAppName() {
        return appName;
    }

    public String getAppVersion() {
        return appVersion;
    }

    public String getAppDescription() {
        return appDescription;
    }
}
```


#### 4. Serviço (ConfigService.java)

```java
package com.example.service;

import com.example.model.ConfigProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ConfigService {

    @Autowired
    private ConfigProperties configProperties;

    public String getAppDetails() {
        return String.format("App: %s, Version: %s, Description: %s",
                configProperties.getAppName(),
                configProperties.getAppVersion(),
                configProperties.getAppDescription());
    }
}
```


#### 5. Controlador (ConfigController.java)

```java
package com.example.controller;

import com.example.service.ConfigService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/config")
public class ConfigController {

    @Autowired
    private ConfigService configService;

    @GetMapping
    public String getAppConfig() {
        return configService.getAppDetails();
    }
}
```

---


### Explicação do Código

#### Classe Principal (Application.java):

- Inicializa a aplicação Spring Boot usando **SpringApplication.run**.


#### Arquivo de Configuração (application.properties):

- Define propriedades da aplicação (**app.name, app.version, app.description**) que serão injetadas nos componentes Spring.


#### Modelo (ConfigProperties.java):

- Anotada com @**Component** para ser gerenciada pelo Spring.

- Usa @**Value** para injetar as propriedades definidas em **application.properties**.


#### Serviço (ConfigService.java):

- Usa **ConfigProperties** para acessar as propriedades injetadas e formatar uma string com os detalhes da aplicação


#### Controlador (ConfigController.java):

- Define um endpoint **/api/config** que retorna os detalhes da aplicação usando o serviço **ConfigService**.


---


#### Exemplos de Uso de @Value

1. **Injeção Simples de String:**
   
- Injetando um valor de string a partir de application.properties.

```java
@Value("${app.name}")
private String appName;
```


2. **Injeção de Número:**

- Injetando um valor numérico e convertendo para o tipo adequado.

```java
@Value("${app.version}")
private String appVersion;
```


3. **Injeção de Listas**

- Injetando uma lista de valores separados por vírgulas.

```properties
app.supportedLocales=en,fr,es
```

```java
@Value("${app.supportedLocales}")
private List<String> supportedLocales;
```


4. **Injeção com Valores Default:**

- Especificando um valor default caso a propriedade não esteja definida.

```java
@Value("${app.description:Default Description}")
private String appDescription;
```


5. **Uso de Expressões SpEL:**

- Usando expressões Spring Expression Language (SpEL) para manipular valores.

```java
@Value("#{${app.version} matches '[0-9]+\\.[0-9]+\\.[0-9]+' ? 'Valid Version' : 'Invalid Version'}")
private String appVersionValidation;
```

---


### Benefícios de Usar @Value

- **Configuração Externa**: Facilita a configuração da aplicação sem a necessidade de alterar o código-fonte.

- **Flexibilidade**: Permite o uso de diferentes fontes de configuração (propriedades, variáveis de ambiente, argumentos de linha de comando).

- **Facilidade de Teste:** Facilita a substituição de valores de configuração para testes.


---


### Considerações

- **Performance**: Evite o uso excessivo de @**Value** em métodos frequentemente chamados, pois a resolução de propriedades pode impactar a performance.


- **Uso em Componentes Gerenciados**: @**Value** só funciona em componentes gerenciados pelo Spring (anotados com @**Component**, @**Service**, etc.).


---


### Conclusão

- A annotation @**Value** é uma ferramenta poderosa no Spring para injetar valores de propriedades externas em componentes da aplicação, promovendo a configuração flexível e a aderência às boas práticas de arquitetura limpa e código limpo. Ela permite que as aplicações sejam configuráveis e adaptáveis sem a necessidade de recompilação ou mudanças no código-fonte.


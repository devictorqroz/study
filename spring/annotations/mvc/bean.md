
# Annotations

## @Bean

A annotation @**Bean** é fundamental no Spring Framework para **definir** e **configurar** **beans** diretamente no código Java.

Ela é usada dentro de classes configuradas com @**Configuration** para indicar que um **método produz um bean** gerenciado pelo Spring.


### Definição Básica

A annotation @**Bean** é aplicada a **métodos** dentro de uma classe anotada com @**Configuration**. 

Cada método anotado com @**Bean** produz um **bean** gerenciado pelo Spring, que é **registrado** no **contexto** da aplicação.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public UserService userService() {
        return new UserService(userRepository());
    }

    @Bean
    public UserRepository userRepository() {
        return new UserRepository();
    }
}
```

---

### Propósito e Uso

- **Definir Beans:** @**Bean** é usado para **declarar** beans **explicitamente** dentro do código Java. Isso é útil quando você precisa de **controle programático** sobre a **criação** de **beans** ou precisa de lógica complexa para criar ou **configurar** o bean.


- **Configuração Centralizada:** Classes anotadas com @**Configuration** e métodos @**Bean** permitem **centralizar** a configuração e a definição de beans em um local, facilitando a gestão e a leitura da configuração da aplicação.


- **Interação com o Container Spring:** @**Bean** permite que o Spring gerencie o **ciclo de vida dos objetos**, incluindo a injeção de dependências, inicialização e destruição de beans.

---

### Exemplo de Uso

#### Classe de Configuração

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public UserService userService() {
        return new UserService(userRepository());
    }

    @Bean
    public UserRepository userRepository() {
        return new UserRepository();
    }
}
```


#### Inicialização de Aplicação

Para usar a configuração, você inicializa o contexto Spring com a classe de configuração.

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {

    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        
        UserService userService = context.getBean(UserService.class);
        userService.performService();
    }
}
```

---

### Atributos da Annotation @Bean

- **name:** Especifica um ou mais nomes para o bean. Se não for especificado, o nome do método será usado como nome do bean.

```java
@Bean(name = "myUserService")
public UserService userService() {
    return new UserService(userRepository());
}
```


- **initMethod:** Especifica o método de inicialização a ser chamado após a criação do bean.

```java
@Bean(initMethod = "init")
public UserService userService() {
    return new UserService(userRepository());
}
```


- **destroyMethod:** Especifica o método de destruição a ser chamado antes da remoção do bean.

```java
@Bean(destroyMethod = "cleanup")
public UserService userService() {
    return new UserService(userRepository());
}
```

---

### Scoping de Beans

- Por padrão, beans definidos com @**Bean** são **singleton**, mas você pode especificar outros escopos como **prototype**, **request**, **session**, etc., usando a annotation @**Scope**.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
public class AppConfig {

    @Bean
    @Scope("prototype")
    public UserService userService() {
        return new UserService(userRepository());
    }

    @Bean
    public UserRepository userRepository() {
        return new UserRepository();
    }
}
```

---

### Integração com Component Scanning

- Enquanto @**Bean** é usada para definir beans **explicitamente**, o Spring também suporta a **detecção automática** de beans usando **component** **scanning** com estereótipos como @**Component**, @**Service**, @**Repository** e @**Controller**. 


- Ambos os métodos podem ser **usados em conjunto** para configurar o contexto da aplicação.


---

### Exemplo Completo

#### Classe de Configuração com @Bean e @ComponentScan

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {

    @Bean
    public UserService userService() {
        return new UserService(userRepository());
    }

    @Bean
    public UserRepository userRepository() {
        return new UserRepository();
    }
}
```

#### Uso na Aplicação

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {

    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        
        UserService userService = context.getBean(UserService.class);
        userService.performService();
    }
}
```

---

### Conclusão

- A annotation @**Bean** é uma poderosa ferramenta no Spring Framework para definir e **configurar** beans **explicitamente** no código Java.


- Ela oferece **controle programático** sobre a criação e configuração de beans, **centraliza** a configuração e facilita a gestão de dependências complexas.



# Configuration Java


Il est possible de configurer Spring sans fichier XML.
Il faut dans ce cas construire le contexte avec une classe dédiée (par exemple _AnnotationConfigApplicationContext_) :

```java
@Component
public class PizzaService {
    public String getVersion() {
        return "1.0";
    }
}
```

```java
@Configuration
@ComponentScan("fr.pizzeria.spring")
public class AppConfig {
}
```

```java
try (AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class)) {
    PizzaService pizzaService = context.getBean(PizzaService.class);
    assertNotNull(pizzaService);
    assertEquals("1.0", pizzaService.getVersion());
}
```

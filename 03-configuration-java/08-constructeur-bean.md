# Constructeur de bean


* **@Bean** définit un bean.

* L'identifiant du bean est déterminé par le nom de la méthode.

* L'injection est effectuée dans le corps de la méthode.

```java
@Configuration
public class AppConfig {
    
    @Bean
    public PizzaService pizzaService() { 
        return new PizzaService();
    }
    
    @Bean
    public PizzaRepository pizzaRepo() { 
        return new PizzaRepository();
    }
}
```

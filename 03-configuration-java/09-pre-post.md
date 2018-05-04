# Traitements pré-post-processeurs

```java
@Component("pizzaService")
@Scope("prototype")
public class PizzaServiceImpl implements IPizzaService {
    @PostConstruct
    public void init() {
        // du code exécuté juste après la création du bean
    }
    @PreDestroy
    public void destroy() {
         // du code exécuté avant la destruction du bean
    }
}
```
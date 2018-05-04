# @Value


```java
@Component
public class PizzaService {
    @Autowired
    private PizzaRepository pizzaRepository;

    // @Value définit une valeur pour un bean.
    // il est possible d'utiliser une variable du contexte Spring
    @Value("${jdbc.user}")
    public void setUser(String user) {
        this.user = user;
    }
}
```
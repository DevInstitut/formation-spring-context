# @Qualifier

```java
@Component
public class PizzaService {
    @Autowired
    // l'annotation @Qualifier permet de spécifier l'identifiant d'un bean
    // il est utile par exemple quand il existe plusieurs beans de même type
    @Qualifier("pizzaRepo1")
    private PizzaRepository pizzaRepository;

    public String findAll() {
        return pizzaRepository.findAll();
    }
}
```


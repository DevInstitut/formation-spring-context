# Injecter une dépendance par type


```java
// L'annotation @Component définit un bean
@Component
public class PizzaService {

    // L'annotation @Autowired permet d'injecter une dépendance
    // Il doit exister dans le contexte Spring un bean de type PizzaRepository
    @Autowired
    private PizzaRepository pizzaRepository;

    public String findAll() {
        return pizzaRepository.findAll();
    }
}
```

# @Value

`@Value` sert à injecter une valeur via un *setter* ou un champ.

Il est notamment souvent utilisé pour récupérer des valeurs depuis un fichier de propriété.

Exemple de fichier de propriété `app.properties` :

```properties
jdbc.user=helene
```


```java
@PropertySource("app.properties")
public class AppConfig {
    
    @Value("${jdbc.user}")
    private String username;
    
}


```
# @Value

`@Value` sert à injecter une valeur via un *setter* ou un champ.

Il est notamment souvent utilisé pour récupérer des valeurs depuis un fichier de propriété.

Exemple de fichier de propriété `app.properties` :

```properties
jdbc.user=helene
```

Exemple de lecture du fichier et récupération des valeurs du fichier.


```java
// Import du contenu du fichier
@PropertySource("app.properties")
public class AppConfig {
    
    // injection de la valeur
    @Value("${jdbc.user}")
    private String username;
    
}


```
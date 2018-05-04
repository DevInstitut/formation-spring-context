# Activer le scan des packages

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>

    <!-- Rechercher des beans dans le package fr.pizzeria.spring -->
    <context:component-scan base-package="fr.pizzeria.spring"></context:component-scan>

</beans>
```
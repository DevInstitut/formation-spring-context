# Portée

Spring définit pour chaque bean sa portée.

Les portées disponibles :

* `singleton` : une seule instance par contexte. C'est la portée par défaut
* `prototype` : une nouvelle instance est créée chaque fois que le bean est injecté
* `session` : une nouvelle instance créée par session utilisateur (environnement Web)
* `request` : une nouvelle instance créée pour chaque requête (environnement Web)
* `application` : une nouvelle instance par ServletContext (environnement Web)
* `custom` : scope personnalisé.

```xml
<beans>

    <!--  L'attribut scope définit le cycle de vie du bean  -->
    <bean id="humanSrv" class="dev.beans.HumanSrv" scope="prototype"/>

</beans>
```
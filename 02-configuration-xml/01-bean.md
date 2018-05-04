# Configurer un bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- Création d'un bean Spring (singleton par défaut) -->
    <bean id="humanSrv" class="dev.beans.HumanSrv"/>

</beans>
```

Equivalent Java

```java
HumanSrv humanSrv = new HumanSrv();
```

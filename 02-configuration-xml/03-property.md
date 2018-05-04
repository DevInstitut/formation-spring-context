# `property`

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="animalSrv" class="dev.beans.AnimalSrv"/>
    <bean id="humanSrv" class="dev.beans.HumanSrv"/>

    <bean id="worldSrv" class="dev.beans.WorldSrv">
        <constructor-arg name="humanSrv" ref="humanSrv"/>

        <property name="animalSrv" ref="animalSrv"/>
    </bean>
</beans>
```

Equivalent Java

```java
HumanSrv humanSrv = new HumanSrv();
WorldSrv worldServ = new WorldSrv(humanSrv);
AnimalSrv animalSrv = new AnimalSrv();

worldServ.setAnimalSrv(animalSrv);
```
# constructor-arg


```xml
<beans>

    <bean id="humanSrv" class="dev.beans.HumanSrv"/>
    
    <bean id="worldServ" class="dev.beans.WorldSrv">
        <constructor-arg name="humanSrv" ref="humanSrv"/>
    </bean>

</beans>
```

Equivalent Java

```java
HumanSrv humanSrv = new HumanSrv();

WorldSrv worldServ = new WorldSrv(humanSrv);
```
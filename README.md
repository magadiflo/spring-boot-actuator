# [Spring Boot Actuator: El Spring Boot Starter número uno que debes incluir en cada aplicación](https://www.youtube.com/watch?v=4OVe0MWgZ4k&t=181s)

Tutorial tomado del canal de **youtube de Dan Vega.**

---

En este tutorial analizaremos por qué `Spring Boot Actuator` debe ser el primer `Spring Boot Starter` que debe agregar
en cada aplicación.

---

## Dependencias

````xml
<!--Spring Boot 3.2.5-->
<!--Java 21-->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
````

`Spring Boot Actuator`, admite puntos finales integrados (o personalizados) que le permiten monitorear y administrar su
aplicación, como el estado de la aplicación, métricas, sesiones, etc.

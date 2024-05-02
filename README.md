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

## Spring Boot Actuator

Spring Boot incluye una serie de características adicionales para ayudarlo a monitorear y administrar su aplicación
cuando la lleva a producción. Puede optar por administrar y monitorear su aplicación mediante puntos finales HTTP o con
JMX. La auditoría, el estado y la recopilación de métricas también se pueden aplicar automáticamente a su aplicación.

`Spring Boot Actuator`, admite puntos finales integrados (o personalizados) que le permiten monitorear y administrar su
aplicación, como el estado de la aplicación, métricas, sesiones, etc.

El módulo `spring-boot-actuator` proporciona todas las funciones listas para producción de `Spring Boot`. La
forma recomendada de habilitar las funciones es agregar la dependencia del `"Starter"` `spring-boot-starter-actuator`.

## Endpoints

Los puntos finales del actuador le permiten monitorear e interactuar con su aplicación. Spring Boot incluye varios
puntos finales integrados y le permite agregar los suyos propios. Por ejemplo, el punto final de estado proporciona
información básica sobre el estado de la aplicación.

Puede habilitar o deshabilitar cada punto final individual y exponerlos (hacerlos accesibles de forma remota) a través
de HTTP o JMX. Se considera que un punto final está disponible cuando está habilitado y expuesto. Los puntos finales
integrados se configuran automáticamente solo cuando están disponibles. La mayoría de las aplicaciones eligen la
exposición en lugar de HTTP, donde el ID del punto final y el prefijo `/actuador` se asignan a una URL. Por ejemplo, de
forma predeterminada, el punto final de salud se asigna a `/actuator/health`.

## Ejecutando aplicación

Si ejecutamos la aplicación, tan solo habiendo agregado las dependencias, veremos en consola lo siguiente:

````bash
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.2.5)

main] d.m.a.app.SpringBootActuatorApplication  : Starting SpringBootActuatorApplication using Java 21.0.1 with PID 12096 (M:\PROGRAMACION\DESARROLLO_JAVA_SPRING\02.youtube\15.dan_vega\spring-boot-actuator\target\classes started by USUARIO in M:\PROGRAMACION\DESARROLLO_JAVA_SPRING\02.youtube\15.dan_vega\spring-boot-actuator)
main] d.m.a.app.SpringBootActuatorApplication  : No active profile set, falling back to 1 default profile: "default"
main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 8080 (http)
main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.20]
main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 2549 ms
main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 1 endpoint(s) beneath base path '/actuator'
main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path ''
main] d.m.a.app.SpringBootActuatorApplication  : Started SpringBootActuatorApplication in 4.905 seconds (process running for 5.675)
````

Del log anterior, centrémonos en el siguiente mensaje que nos
muestra: `Exposing 1 endpoint(s) beneath base path '/actuator'`, **¿y eso qué significa?**, pues si abrimos en el
navegador la url veremos algunos endpoints que Actuator nos proporciona.

En mi caso, usaré la línea de comando para abrir dicha url:

````bash
$ curl -v http://localhost:8080/actuator | jq
>
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 00:30:34 GMT
<
{
  "_links": {
    "self": {
      "href": "http://localhost:8080/actuator",
      "templated": false
    },
    "health": {
      "href": "http://localhost:8080/actuator/health",
      "templated": false
    },
    "health-path": {
      "href": "http://localhost:8080/actuator/health/{*path}",
      "templated": true
    }
  }
}
````

Accedemos al path `/health`, nos mostrará el `status: UP`, es decir, que esta aplicación está activo, su **salud** es
buena. Este `status` va a ser un indicador para que otras aplicaciones llamen y averiguen si nuestro estado está activo.

````bash
$ curl -v http://localhost:8080/actuator/health | jq
>
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 00:32:59 GMT
<
{
  "status": "UP"
}
````
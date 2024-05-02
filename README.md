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

## [Endpoints](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html?query=health%27%20target=_blank%3E%3Cb%3Ehealth%3C/b%3E%3C/a%3E-groups)

Los puntos finales del actuador le permiten monitorear e interactuar con su aplicación. Spring Boot incluye varios
puntos finales integrados y le permite agregar los suyos propios. Por ejemplo, el punto final de estado proporciona
información básica sobre el estado de la aplicación.

Puede habilitar o deshabilitar cada punto final individual y exponerlos (hacerlos accesibles de forma remota) a través
de HTTP o JMX. Se considera que un punto final está disponible cuando está habilitado y expuesto. Los puntos finales
integrados se configuran automáticamente solo cuando están disponibles. La mayoría de las aplicaciones eligen la
exposición en lugar de HTTP, donde el ID del punto final y el prefijo `/actuador` se asignan a una URL. Por ejemplo, de
forma predeterminada, el punto final de salud se asigna a `/actuator/health`.

Están disponibles los siguientes puntos de conexión independientes de la tecnología:

- `beans`, muestra una lista completa de todos los frijoles de primavera de la aplicación.
- `info`, muestra información arbitraria de la aplicación.
- `logfile`, devuelve el contenido del archivo de registro (si se ha establecido la propiedad `logging.file.name`
  o `logging.file.path`). Admite el uso del encabezado de rango HTTP para recuperar parte del contenido del archivo de
  registro.
- etc..

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

## Habilitando algunos endpoints

Por defecto, `Spring Boot Actuator` nos muestra 3 endpoints habilitados, tal como se ve en el apartado superior:

````bash
http://localhost:8080/actuator
http://localhost:8080/actuator/health
http://localhost:8080/actuator/health/{*path}
````

Ahora, nosotros habilitaremos algunos endpoints adicionales para obtener información que nos interesa.

````yaml
spring:
  application:
    name: spring-boot-actuator

management:
  endpoints:
    web:
      exposure:
        include: health, beans
````

Accediendo al endpoint de `/actuator` vemos que ahora nos muestra el endpoint `/bean` configurado:

````bash
$ curl -v http://localhost:8080/actuator | jq
>
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 00:59:08 GMT
<
{
  "_links": {
    "self": {
      "href": "http://localhost:8080/actuator",
      "templated": false
    },
    "beans": {
      "href": "http://localhost:8080/actuator/beans",
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

## Viendo todos los Beans definidos en el Contexto de la aplicación

````bash
$ curl -v http://localhost:8080/actuator/beans | jq
>
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 01:02:40 GMT
{
    "contexts": {
        "spring-boot-actuator": {
            "beans": {
                "endpointCachingOperationInvokerAdvisor": {
                    "aliases": [],
                    "scope": "singleton",
                    "type": "org.springframework.boot.actuate.endpoint.invoker.cache.CachingOperationInvokerAdvisor",
                    "resource": "class path resource [org/springframework/boot/actuate/autoconfigure/endpoint/EndpointAutoConfiguration.class]",
                    "dependencies": [
                        "org.springframework.boot.actuate.autoconfigure.endpoint.EndpointAutoConfiguration",
                        "environment"
                    ]
                },
                "defaultServletHandlerMapping": {
                    "aliases": [],
                    "scope": "singleton",
                    "type": "org.springframework.web.servlet.HandlerMapping",
                    "resource": "class path resource [org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration$EnableWebMvcConfiguration.class]",
                    "dependencies": [
                        "org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration$EnableWebMvcConfiguration"
                    ]
                },
                "applicationTaskExecutor": {
                    "aliases": [
                        "taskExecutor"
                    ],
                    "scope": "singleton",
                    "type": "org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor",
                    "resource": "class path resource [org/springframework/boot/autoconfigure/task/TaskExecutorConfigurations$TaskExecutorConfiguration.class]",
                    "dependencies": [
                        "org.springframework.boot.autoconfigure.task.TaskExecutorConfigurations$TaskExecutorConfiguration",
                        "taskExecutorBuilder"
                    ]
                },
                {...}
                "httpMessageConvertersRestClientCustomizer": {
                    "aliases": [],
                    "scope": "singleton",
                    "type": "org.springframework.boot.autoconfigure.web.client.HttpMessageConvertersRestClientCustomizer",
                    "resource": "class path resource [org/springframework/boot/autoconfigure/web/client/RestClientAutoConfiguration.class]",
                    "dependencies": [
                        "org.springframework.boot.autoconfigure.web.client.RestClientAutoConfiguration"
                    ]
                },
                "taskExecutorBuilder": {
                    "aliases": [],
                    "scope": "singleton",
                    "type": "org.springframework.boot.task.TaskExecutorBuilder",
                    "resource": "class path resource [org/springframework/boot/autoconfigure/task/TaskExecutorConfigurations$TaskExecutorBuilderConfiguration.class]",
                    "dependencies": [
                        "org.springframework.boot.autoconfigure.task.TaskExecutorConfigurations$TaskExecutorBuilderConfiguration",
                        "spring.task.execution-org.springframework.boot.autoconfigure.task.TaskExecutionProperties"
                    ]
                },
                "org.springframework.boot.actuate.autoconfigure.endpoint.jackson.JacksonEndpointAutoConfiguration": {
                    "aliases": [],
                    "scope": "singleton",
                    "type": "org.springframework.boot.actuate.autoconfigure.endpoint.jackson.JacksonEndpointAutoConfiguration",
                    "dependencies": []
                }
            }
        }
    }
}
````

## Habilitando todos los endpoints

En lugar de estar definiendo los endpoints, podemos habilitar todos los endpoints de una vez colocando el `*`:

````yaml
# Another property
#
management:
  endpoints:
    web:
      exposure:
        include: '*'
````

Ahora accedemos al endpoint y vemos que estarán habilitados todos los endpoints:

````bash
$ curl -v http://localhost:8080/actuator | jq
>
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 01:09:53 GMT
<
{
  "_links": {
    "self": {
      "href": "http://localhost:8080/actuator",
      "templated": false
    },
    "beans": {
      "href": "http://localhost:8080/actuator/beans",
      "templated": false
    },
    "caches-cache": {
      "href": "http://localhost:8080/actuator/caches/{cache}",
      "templated": true
    },
    "caches": {
      "href": "http://localhost:8080/actuator/caches",
      "templated": false
    },
    "health": {
      "href": "http://localhost:8080/actuator/health",
      "templated": false
    },
    "health-path": {
      "href": "http://localhost:8080/actuator/health/{*path}",
      "templated": true
    },
    "info": {
      "href": "http://localhost:8080/actuator/info",
      "templated": false
    },
    "conditions": {
      "href": "http://localhost:8080/actuator/conditions",
      "templated": false
    },
    "configprops": {
      "href": "http://localhost:8080/actuator/configprops",
      "templated": false
    },
    "configprops-prefix": {
      "href": "http://localhost:8080/actuator/configprops/{prefix}",
      "templated": true
    },
    "env": {
      "href": "http://localhost:8080/actuator/env",
      "templated": false
    },
    "env-toMatch": {
      "href": "http://localhost:8080/actuator/env/{toMatch}",
      "templated": true
    },
    "loggers-name": {
      "href": "http://localhost:8080/actuator/loggers/{name}",
      "templated": true
    },
    "loggers": {
      "href": "http://localhost:8080/actuator/loggers",
      "templated": false
    },
    "heapdump": {
      "href": "http://localhost:8080/actuator/heapdump",
      "templated": false
    },
    "threaddump": {
      "href": "http://localhost:8080/actuator/threaddump",
      "templated": false
    },
    "metrics-requiredMetricName": {
      "href": "http://localhost:8080/actuator/metrics/{requiredMetricName}",
      "templated": true
    },
    "metrics": {
      "href": "http://localhost:8080/actuator/metrics",
      "templated": false
    },
    "scheduledtasks": {
      "href": "http://localhost:8080/actuator/scheduledtasks",
      "templated": false
    },
    "mappings": {
      "href": "http://localhost:8080/actuator/mappings",
      "templated": false
    }
  }
}
````

## Viendo más detalles del path /health

Tenemos que agregar la siguiente configuración para ver más detalles sobre el entpoint `/healt`:

````yaml
# another configuration
#
management:
  # another configuration
  #
  endpoint:
    health:
      show-details: always
````

````bash
$ curl -v http://localhost:8080/actuator/health | jq
>
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 01:13:31 GMT
{
  "status": "UP",
  "components": {
    "diskSpace": {
      "status": "UP",
      "details": {
        "total": 240053645312,
        "free": 97483169792,
        "threshold": 10485760,
        "path": "M:\\PROGRAMACION\\DESARROLLO_JAVA_SPRING\\02.youtube\\15.dan_vega\\spring-boot-actuator\\.",
        "exists": true
      }
    },
    "ping": {
      "status": "UP"
    }
  }
}
````

## Agregando configuraciones al endpoint /info

Agregamos la siguiente configuración en el `application.yml`:

````yaml
management:
  # another configuration
  #
  info:
    env:
      enabled: true

info:
  app:
    name: spring-boot-actuator
    description: Event Managament Application
    version: 1.0.0
    author: Martin
    docs: http://magadiflo.dev
````

Accedemos al endpoint `/info` y veremos que se muestra nuestra información personalizada:

````bash
$  curl -v http://localhost:8080/actuator/info | jq
>
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 01:19:09 GMT
<
{
  "app": {
    "name": "spring-boot-actuator",
    "description": "Event Managament Application",
    "version": "1.0.0",
    "author": "Martin",
    "docs": "http://magadiflo.dev"
  }
}
````

Podemos agregar nuevas configuraciones para ver más información

````yaml
management:
  endpoints:
    web:
      exposure:
        include: '*'
  endpoint:
    health:
      show-details: always
  info:
    env:
      enabled: true
    build:
      enabled: true
    git:
      enabled: true
      mode: full
    java:
      enabled: true
    os:
      enabled: true
````

````bash
$  curl -v http://localhost:8080/actuator/info | jq
<
< HTTP/1.1 200
< Content-Type: application/vnd.spring-boot.actuator.v3+json
< Transfer-Encoding: chunked
< Date: Thu, 02 May 2024 01:23:58 GMT
<
{
  "app": {
    "name": "spring-boot-actuator",
    "description": "Event Managament Application",
    "version": "1.0.0",
    "author": "Martin",
    "docs": "http://magadiflo.dev"
  },
  "java": {
    "version": "21.0.1",
    "vendor": {
      "name": "Oracle Corporation"
    },
    "runtime": {
      "name": "Java(TM) SE Runtime Environment",
      "version": "21.0.1+12-LTS-29"
    },
    "jvm": {
      "name": "Java HotSpot(TM) 64-Bit Server VM",
      "vendor": "Oracle Corporation",
      "version": "21.0.1+12-LTS-29"
    }
  },
  "os": {
    "name": "Windows 11",
    "version": "10.0",
    "arch": "amd64"
  }
}
````
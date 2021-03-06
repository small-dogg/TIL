# Service Discovery: Eureka Server

## ๐ How to include Eureka server

ํ๋ก์ ํธ์ Eureka Server๋ฅผ ์ถ๊ฐํ๊ธฐ ์ํด, `org.springframework.cloud` Group ID์
spring-cloud-`starter-netflix-eureka-server` artifact ID๋ฅผ ์ถ๊ฐํ์ธ์.

**application.yaml**

```yaml
spring:
  freemarker:
    template-loader-path: classpath:/templates/
    prefer-file-system-access: false
```

## ๐ How to Run a Eureka Server

์๋๋ Eureka server๋ฅผ ๋์์ํค๊ธฐ ์ํ ์ต์ํ์ ๋ฐฉ๋ฒ์ ๋ณด์ฌ์ค๋๋ค:

```java
@SpringBootApplication
@EnableEurekaServer
public class Application {
	new SpringApplicationBuilder(Application.class).web(true).run(args);
}
```

Eureka Server๋ ๊ธฐ๋ณธ์ ์ผ๋ก Eureka ๊ธฐ๋ฅ์ ์ํ UI ๋ฐ HTTP API endpoint๋ฅผ ์ ๊ณตํ๋ `/eureka/*` ํํ์ด์ง๋ฅผ ์ ๊ณตํฉ๋๋ค.

>๐ก Gradle์ ์ข์์ฑ ํด๊ฒฐ๊ท์น ๋ฐ ๋ถ๋ชจ bom ๊ธฐ๋ฅ์ ๋ถ์ฌ๋ก ์ธํด, `spring-cloud-starter-netflix-eureka-server`๊ฐ application์์ ์คํ๋์ง ์์ ์ ์์ต๋๋ค. ๋ฐ๋ผ์, ์ด๋ฌํ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด, Spring cloud starter ๋ถ๋ชจ bom์ importํ๊ณ , Spring Boot Gradle plugin์ ์ถ๊ฐํด์ผ ํฉ๋๋ค.


```groovy
buildscript {
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:{spring-boot-docs-version}")
    }
}

apply plugin: "spring-boot"

dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:{spring-cloud-version}"
    }
}
```

## ๐ High Availability, Zones and Regions

Eureka server๋ backend ์ ์ฅ์๋ฅผ ๊ฐ์ง๊ณ  ์์ง ์์ต๋๋ค. ํ์ง๋ง, ๋ ์ง์คํธ๋ฆฌ ์์ ๋ชจ๋  service instance๋ ๊ทธ๋ค์ ํ์ฌ ์ํ๋ฅผ ๊ณ์์ ์ผ๋ก ํ์ธํ  ์
์๋๋ก ํํธ๋นํธ๋ฅผ ์ ์กํด์ผ ํฉ๋๋ค(๊ทธ๋์, ๋ฉ๋ชจ๋ฆฌ์์ ์ํํ  ์ ์์). Client๋ ๋ํ Eureka ๋ฑ๋ก์ ์ธ-๋ฉ๋ชจ๋ฆฌ ์บ์๋ฅผ ๊ฐ์ง๊ณ  ์์ต๋๋ค(๋ฐ๋ผ์ ์๋น์ค์ ๋ํ ๋ชจ๋  ์์ฒญ์
๋ํด ๋ ์ง์คํธ๋ฆฌ๋ก ์ด๋ํ  ํ์๊ฐ ์์).

๊ธฐ๋ณธ์ ์ผ๋ก, ๋ชจ๋  Eureka Server๋ Eureka Client ์ด๊ธฐ๋ํ๋ฉฐ, ๋์์ ํผ์ด๋ฅผ ์ฐพ๊ธฐ ์ํ Service URL์ด ํ์ํฉ๋๋ค. ๋ง์ฝ Service URL์ ์ ๊ณตํ์ง
์์ ๊ฒฝ์ฐ, ์๋น์ค๋ ๋์ํ๊ฒ ์ง๋ง, ํผ์ด๋ฅผ ๋ฑ๋กํ  ์ ์๋ค๋ ๋ด์ฉ์ ๋ก๊ทธ๊ฐ ๊ณ์์ ์ผ๋ก ์ฆ์ ๋  ๊ฒ์๋๋ค.

## ๐ Standalone Mode

2๊ฐ์ ์บ์(Server ๋ฐ Client)์ ํํธ๋นํธ์ ์กฐํฉ์ ๋ชจ๋ํฐ๋ง์ด ์ ์ง๋๊ฑฐ๋ Elastic runtime(Cloud Foundry ๊ฐ์..)์ด ์ ์ง๋๋ํ standalone
Eureka Server๋ฅผ ํ๋ ฅ์ ์ผ๋ก ๋ง๋ญ๋๋ค. standalone ๋ชจ๋์์๋ ํด๋ผ์ด์ธํธ ์ธก ๋์์ ํด์ ํ์ฌ ํผ์ด์ ๊ณ์์ ์ผ๋ก ์ฐ๊ฒฐํ๋ ํ์๋ฅผ ํ์ง ์๋๋ก ํ  ์ ์์ต๋๋ค. ๋ค์
์์๋ ํด๋ผ์ด์ธํธ ์ธก ๋์(client-side behavior)๋ฅผ ๋๋ ๋ฐฉ๋ฒ์ ๋ณด์ฌ์ค๋๋ค.

**application.yaml**

```yaml
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

## ๐ Peer Awareness

Eureka๋ ๋์์ค์ธ ๋ค์ค ์ธ์คํด์ค๋ฅผ ์คํํ๊ณ , ์๋ก ๊ฐ๊ฐ์๊ฒ ๋ฑ๋กํ๋๋ก ์์ฒญํ์ฌ ๋ณด๋ค ํ๋ ฅ์ ์ผ๋ก ์ด์ฉํ  ์ ์์ต๋๋ค. ์๋์ ์์ ์ ๊ฐ์ด ํผ์ด๋ฅผ ๋์์ผ๋ก
์ ํจํ `serviceUrl`์ ์ถ๊ฐํ๊ธฐ๋ง ํ๋ฉด, ๊ตฌ์ฑ์ด ์๋ฃ๋ฉ๋๋ค.

**application.yaml (Two Peer Aware Eureka Servers)**

```yaml
---
spring:
  profiles: peer1
eureka:
  instance:
    hostanme: peer1
  client:
    serviceUrl:
      defaultZone: https://peer2/eureka/

---
spring:
  profiles: peer2
eureka:
  instance:
    hostname: peer2
  client:
    serviceUrl:
      defaultZone: https://peer1/eureka/
```

์์ ์์์์๋ ์๋ก ๋ค๋ฅธ ๋ Peer๊ฐ ์๋ก๋ฅผ ์ธ์ํ์ฌ, ๋์ผํ Server ํ๊ฒฝ์ ๋ง๋ค ์ ์๋ ์์์์ต๋๋ค. `eureka.instance.hostname`
์ `/etc/hosts` ํ์ผ๋ก ๋ถํฐ ์๊ธฐ ์์ ์ ํธ์คํธ ๋ช์ ์ธ์งํ  ์ ์๋ OS ํ๊ฒฝ์์๋ ์ฌ์ฉํ์ง ์์๋ ๋ฉ๋๋ค.

์์คํ์ ์ฌ๋ฌ ํผ์ด๋ฅผ ์ถ๊ฐํ  ์ ์์ผ๋ฉฐ, ๋ชจ๋ ์ต์ํ ํ๋์ edge๋ก ์๋ก ์ฐ๊ฒฐ๋์ด์์ผ๋ฉด ์๋ก๊ฐ ๋ฑ๋ก์ ํตํด ๋๊ธฐํํ  ์ ์์ต๋๋ค. ํผ์ด๊ฐ ๋ฌผ๋ฆฌ์ ์ผ๋ก ๋ถ๋ฆฌ๋์ด์๋ ๊ฒฝ์ฐ
split-brain ์ด ๋ฐ์ํ๋ ๊ฒฝ์ฐ๋ฅผ ๋๋นํ  ์ ์์ต๋๋ค. ์ฌ๋ฌ ํผ์ด๋ฅผ ์์คํ์ ์ถ๊ฐํ  ์ ์์ผ๋ฉฐ, ๋ชจ๋ ์๋ก์๊ฒ ์ง์  ์ฐ๊ฒฐ๋์ด์์ผ๋ฉด ์๋ก ๋ฑ๋ก์ ๋๊ธฐํํฉ๋๋ค.

**application.yaml (Three Peer Aware Eureka Servers)**

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: https://peer1/eureka/,http://peer2/eureka/,http://peer3/eureka/

---
spring:
  profiles: peer1
eureka:
  instance:
    hostname: peer1

---
spring:
  profiles: peer2
eureka:
  instance:
    hostname: peer2

---
spring:
  profiles: peer3
eureka:
  instance:
    hostname: peer3
```

## ๐ When to Prefer IP Address

ํ์์ ๋ฐ๋ผ Eureka ๋ฑ๋ก์ hostname์ด ์๋ IP๋ก ์ฌ์ฉํด์ผํ  ๋๋ ์์ต๋๋ค. `eureka.instance.preferIpAddress`๋ฅผ `true`๋ก ์ค์ ํ๋ฉด,
hostname์ด ์๋ IP๋ฅผ ์ฌ์ฉํฉ๋๋ค.

## ๐ Securing The Eureka Server

`spring-boot-starter-security`๋ฅผ ์๋ฒ์ ClassPath์ ํฌํจํ๊ณ ๋ง ์์ผ๋ฉด๋ฉ๋๋ค. ๊ธฐ๋ณธ์ ์ผ๋ก Spring Seucrity๊ฐ ClassPath์
ํฌํจํ๊ณ ์์ ๊ฒฝ์ฐ, CSRF token์ ๋ณด๋ด๋๋ก ๋์ด์์ต๋๋ค. Eureka Client๋ ์ผ๋ฐ์ ์ผ๋ก ์ ํจํ CSRF(Cross-Site Request Forgery) ํ ํฐ์
์์ ํ๊ณ  ์์ง ์๊ธฐ ๋๋ฌธ์, Spring Security ์ค์ ์ ํตํด, /eureka/** ์๋ํฌ์ธํธ์ ๋ํด ์ด ์๊ตฌ์ฌํญ์ ๋นํ์ฑํ ํด์ผ ํฉ๋๋ค.

```java

@EnableWebSecurity
class WebSecurityConfig extends WebSecurityConfigurerAdapter {

  @Override
  protected void confiure(HttpSecurity http) throws Exception {
    http.csrf().ignoringAntMatchers("/eureka/**");
    super.configure(http);
  }
```

## ๐ JDK 11 Support

Eureka ์๋ฒ๊ฐ ์์กดํ๋ JAXB ๋ชจ๋์ JDK 11์์ ์ ๊ฑฐ๋์์ต๋๋ค. Eureka ์๋ฒ๋ฅผ ์คํํ  ๋ JDK 11์ ์ฌ์ฉํ๋ ค๋ฉด POM ๋๋ Gradle ํ์ผ์ ์ด๋ฌํ ์ข์์ฑ์
ํฌํจํด์ผ ํฉ๋๋ค.
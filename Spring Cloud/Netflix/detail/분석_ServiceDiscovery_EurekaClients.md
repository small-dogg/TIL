# Service Discovery:Eureka Clients

---

## ๐ How to Include Eureka Client

Eureka Client ๋ฑ๋ก์ ์์ฃผ ๊ฐ๋จํฉ๋๋ค. `org.springframework.cloud` Group ID์
`spring-cloud-starter-netflix-eureka-client` Artifact ID๋ฅผ Build System์ ์ถ๊ฐํ์ธ์.

| Group ID | org.springframework.cloud |
| --- | --- |
| Artifact ID | spring-cloud-starter-netflix-eureka-client |

## ๐ Registering with Eureka

Client๊ฐ Eureka๋ฅผ ๋ฑ๋กํ๋ฉด, host, port, health indicator URL, home page ๊ทธ๋ฆฌ๊ณ  ์ธ๋ถ ์ฌํญ ๋ฑ์ ํด๋นํ๋ Client์ ๋ฉํ ๋ฐ์ดํฐ๋ฅผ
์ ๊ณตํฉ๋๋ค. Eureka๋ ์๋น์ค์ ํด๋นํ๋ ๊ฐ๊ฐ์ ์ธ์คํด์ค๋ก๋ถํฐ heartbeat๋ฉ์์ง๋ฅผ ๋ฐ์ต๋๋ค. ๋ง์ฝ, heartbeat๊ฐ configurable timetable์ ํตํด
์ฅ์  ์กฐ์น๋์๋ค๊ณ  ํ๋จ๋๋ฉด, ์ธ์คํด์ค๋ ์ผ๋ฐ์ ์ผ๋ก ๋ ์ง์คํธ๋ฆฌ์์ ์ ๊ฑฐ๋ฉ๋๋ค.

์๋๋ Eureka Client Application์ ๊ตฌ์ฑํ๋ ๊ฐ์ฅ ๊ฐ๋จํ ์์์๋๋ค

```java

@SpringBootApplication
@RestController
public class Application {

  @RequestMapping("/")
  public String home() {
    return "Hello world";
  }

  public static void main(String[] args) {
    new SpringApplicationBuilder(Application.class).web(true).run(args);
  }
}
```

์์ ์์๋ ์ผ๋ฐ์ ์ธ SpringBoot Application๊ณผ ํํ๊ฐ ๋์ผํฉ๋๋ค. ํด๋์ค ๊ฒฝ๋ก์ `spring-cloud-starter-netflix-eureka-client`๊ฐ
์กด์ฌํ๋ฉด, ์ ํ๋ฆฌ์ผ์ด์์ ์๋์ผ๋ก Eureka Server์ Client๋ก ๋ฑ๋ก์ด ๋๋๊ฒ๋๋ค. ๊ทธ๋ฌ๋ฉด, ๋์ Client๊ฐ Eureka Server๋ฅผ ์ฐพ์ ์ ์๋๋ก ๋์์ค์ผ
ํฉ๋๋ค.

**Application.yaml**

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
```

์ ์์์์, `defaultZone`์ ๋ชจ๋  Client์๊ฒ service URL์ ์ ๊ณตํ๋ ๊ฐ์๋๋ค. preference์ ํด๋นํ๋ Zone์ ์์ฑํ์ง ์์ ๊ฒฝ์ฐ,
`defaultZone`์ผ๋ก Eureka Server๊ฐ ๋์ฒด๋ฉ๋๋ค.

์์์ ์ธ๊ธํ ๋ฐ์ ๊ฐ์ด, ClassPath์ `spring-cloud-starter-netflix-eureka-client`๊ฐ ์์ ๊ฒฝ์ฐ ์๋์ผ๋ก Eureka Instance ๋ฐ
Client๋ก ๋ฑ๋ก๋ฉ๋๋ค. Instance์ ๋์์ `eureka.instance.*`์ ํด๋นํ๋ ๊ตฌ์ฑ ํค๋ก ๊ตฌ๋๋๋ฉฐ, `spring.application.name` ๊ฐ์ด
์กด์ฌํ๋ฉด, ๊ตณ์ด ์์ฑํ์ง ์์๋ ๋ฉ๋๋ค.

[**Instance](https://github.com/spring-cloud/spring-cloud-netflix/blob/main/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaInstanceConfigBean.java)
๋ฐ [Client](https://github.com/spring-cloud/spring-cloud-netflix/blob/main/spring-cloud-netflix-eureka-client/src/main/java/org/springframework/cloud/netflix/eureka/EurekaClientConfigBean.java)
๊ตฌ์ฑ ์ต์**

## ๐ Authentication with the Eureka Server

`eureka.client.serviceUrl.defaultZone` URL์ด ์๊ฒฉ์ฆ๋ช์ ํฌํจ๋์ด์์ ๊ฒฝ์ฐ, Eureka Client๋ค์ ์๋์ผ๋ก HTTP ๊ธฐ๋ณธ์ธ์ฆ์ด ์ถ๊ฐ๋ฉ๋๋ค. ๋ง์ฝ
๋ณด๋ค ๋ณต์กํ ์ธ์ฆ์ ์๊ตฌํด์ผํ๋ ๊ฒฝ์ฐ `DiscoveryClientOptionalArgs` ์ ํ์ Bean์ ์์ฑํ๊ณ  `ClientFilter` ์ธ์คํด์ค๋ฅผ ์ฃผ์ํ  ์ ์์ต๋๋ค. ์ด
์ธ์คํด์ค๋ ๋ชจ๋ ํด๋ผ์ด์ธํธ์์ ์๋ฒ๋ก์ ํธ์ถ์ ์ ์ฉ๋ฉ๋๋ค.

๋ง์ฝ, Eureka ์๋ฒ์ ์ธ์ฆ์ ์ํด Client ์ธก ์ธ์ฆ์๊ฐ ํ์ํ ๊ฒฝ์ฐ๋ ์๋์ ๊ฐ์ด ํ๋กํผํฐ๋ฅผ ์์ฑํ์ฌ Client ์ธก ์ธ์ฆ์ ๋ฐ trust store๋ฅผ ๊ตฌ์ฑํ  ์๋
์์ต๋๋ค.

**application.yaml**

```yaml
eureka:
  client:
    tls:
      enabled: true
      key-store: <path-of-key-store>
      key-store-type: PKCS12
      key-store-password: <key-store-password>
      key-password: <key-password>
      trust-store: <path-of-trust-store>
      trust-store-type: PKCS12
      trust-store-password: <trust-store-password>
```

## ๐ Status Page and Health Indicator

Spring Boot Actuator ์ฑ์์ Status ํ์ด์ง์ health indicator๋ ๊ฐ๊ฐ `/info` ์ `/health`๋ก ๊ธฐ๋ณธ ์ค์ ๋ฉ๋๋ค.

๋ง์ฝ ์ด ๊ธฐ๋ณธ ํ์ด์ง๋ฅผ ๋ณ๊ฒฝํ๊ณ  ์ถ๋ค๋ฉด, ์๋์ ๊ฐ์ด ์ค์ ํ๋ฉด ๋ฉ๋๋ค.

```yaml
eureka:
  instance:
    statusPageUrlPath: ${server.servletPath}/info
    healthCheckUrlPath: ${server.servletPath}/health
```

## ๐ Registering a Secure Application

๋ง์ฝ HTTPS๋ฅผ ์ฐ๊ฒฐํ๊ธธ ์ํ๋ค๋ฉด, ์๋์ ๋ ํ๋กํผํฐ๋ฅผ ์ค์ ํ์ธ์.

```java
//EnrekaInstanceConfigBean
eureka.instance.nonSecurePortEnabled=false
    eureka.instance.securePortEnabled=true
```

์์ ๊ฐ์ด ์ค์ ํ๋ฉด, Eureka๊ฐ ์ธ์คํด์ค ์ ๋ณด ์์ ์์ ์ธ์คํด์ค๋ ์ํธ ํต์ ์ ํ์๋ก ํ๋ค๊ณ  ๋ช์์ ์ผ๋ก ๊ธฐ์ฌํฉ๋๋ค. Spring Cloud๋ ์์๊ฐ์ด ์ํธ ํต์ ์ด ํ์๋ก ํ๋
DicoveryClient์ ๋ํด Endpoint URL์ ํญ์ ์ถ๋ ฅํด์ค๋๋ค.

์์ ๊ฐ์ด ์ค์ ์ ํ์ผ๋ฉด, statusPageUrl, healthCheckUrl, homePageUrl์ ๋ํ ํ๋กํผํฐ ๊ฐ๋ ์ ์์ ์ผ๋ก https ํต์ ์ ์ํํ  ์ ์๋๋ก URL์
์์ฑํด์ฃผ์ด์ผํฉ๋๋ค.

```yaml
eureka:
  instance:
    statusPageUrl: https://${eureka.hostname}/info
  healthCheckUrl: https://${eureka.hostname}/health
  homePageUrl: https://${eureka.hostname}/
```

## ๐ Eurekaโs Health Checks

๊ธฐ๋ณธ์ ์ผ๋ก, Eureka๋ heartbeat๋ฅผ ํตํด Client๊ฐ ์ด์์๋์ง์ ์ฌ๋ถ๋ฅผ ํ์ธํฉ๋๋ค. ํน๋ณํ ์ค์ ํ์ง ์์ผ๋ฉด Discvoery Client๋ Spring Boot
Actuator์ ๋ฐ๋ผ ์ ํ๋ฆฌ์ผ์ด์์ ํ์ฌ ์ํ๋ฅผ ์ ๋ฌํ์ง ์์ต๋๋ค. ์ค์ ํ Client์ ๋ํด Eureka๋ Application์ ์ํ๊ฐ โUPโ์ธ์ง์ ์ฌ๋ถ๋ฅผ ์ฒดํฌํฉ๋๋ค.

```yaml
eureka:
  client:
    healthcheck:
      enabled: true
```

com.netflix.appinfo.HeatlCheckHandler์ ๊ตฌํ์ฒด๋ฅผ ์ฐธ๊ณ ํ๋ฉด ๋ ๋ง์ ์ค์  ์ ๋ณด๋ฅผ ํ์ธํ  ์ ์์ต๋๋ค.

## ๐ Eureka Metadata Instances and Clients

### Using Eureka on Cloud Foundry

[https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#using-eureka-on-cloud-foundry](https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#using-eureka-on-cloud-foundry)

### Using Eureka on AWS

[https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#using-eureka-on-aws](https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#using-eureka-on-aws)

## ๐ Changing the Eureka Instance ID

๋ฐ๋๋ผ Netflix Eureka instance๋ host name์ ID๋ก ๋ฑ๋กํฉ๋๋ค. Spring Cloud Eureka๋ ์๋์ ๊ฐ์ ํฉ๋ฆฌ์ ์ธ ๊ธฐ๋ณธ๊ฐ์ ์ ๊ณตํฉ๋๋ค.

${spring.cloud.client.hostname}:${spring.application.name}:${sporing.application.instance_id:
${server.port}}

์๋์ ๊ฐ์ด ํ๋กํผํฐ๋ฅผ ์์ฑํ์ฌ ์ฌ์ ์ํ  ์ ์์ต๋๋ค.

```yaml
eureka:
  instance:
    instanceId: ${spring.application.name}:${vcap.application.instance_id:${spring.application.instance_id:${random.value}}}
```

## ๐ Using the EurekaCleint

ํ๋ ์ด์์ Discovery Client์ธ ์ ํ๋ฆฌ์ผ์ด์์ด ์กด์ฌํ๋ฉด, Eureka Server๋ก๋ถํฐ ์๋น์ค ์ธ์คํด์ค๋ฅผ ๊ฒ์ํ  ์ ์์ต๋๋ค. ๋ค์๊ณผ ๊ฐ์ด
com.netflix.discovery.EurekaClient๋ฅผ ์ฌ์ฉํ๋ฉด ๋ฉ๋๋ค.

```java
@Autowired
private EurekaClient discoveryClient;

public String serviceUrl(){
    InstanceInfo instance-discoveryClient.getNextServerFromEureka("STORES",false);
    return instance.getHomePageUrl();
    }
```

### EurekaClient with Jersey

[https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#eurekaclient-with-jersey](https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#eurekaclient-with-jersey)

## ๐ Alternatives to the Native Netflix EurekaClient

[https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#alternatives-to-the-native-netflix-eurekaclient](https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#alternatives-to-the-native-netflix-eurekaclient)

## ๐ Why Is It so Slow to Register a Service?

๊ธฐ๋ณธ์ ์ผ๋ก, ์ธ์คํด์ค๋ serviceURL์ ํตํด 30์ด ์ฃผ๊ธฐ๋ก ํํธ๋นํธ ํต์ ํ์ฌ ์๋น์ค๋ฅผ ๋ฑ๋กํฉ๋๋ค. ๋ฉํ๋ฐ์ดํฐ๊ฐ ์ผ์นํด์ง๋๊น์ง๋ ์ด๋์ ๋ ์๊ฐ์ด ๊ฑธ๋ฆฌ๊ธฐ๋๋ฌธ์, Server๊ฐ
Client์ ์ ๋ณด๋ฅผ ์์ ํ๋๋ฐ๊น์ง ์ด๋์ ๋ ์๊ฐ์ด ๊ฑธ๋ฆฌ ์ ๋ฐ์ ์์ต๋๋ค. ๋ฌผ๋ก , ์ค์ ๊ฐ์ ํตํด 30์ด ์ฃผ๊ธฐ๋ณด๋ค ๋ ๋น ๋ฅด๊ฒ ์๋น๊ธธ ์๋ ์์ง๋ง, ํ๋ก๋์ ํ๊ฒฝ์์๋ ์ฑ๋ฅ์  ์ธก๋ฉด์
๊ณ ๋ คํ์ฌ ๊ฐ๊ธ์ ์ด๋ฉด ๊ธฐ๋ณธ ์ค์ ์ ๋ฐ๋ฅด๋ ๊ฒ์ ๊ถ๊ณ ํฉ๋๋ค.

```java
eureka.instance.leaseRenewalIntervalInSeconds=30
```

## ๐ Zones

๋ง์ฝ Eureka Client๋ฅผ Multiple zone์ ๋ฐฐํฌํ  ๊ฒฝ์ฐ, ํน์  Zone์ ์ฐ์ ๊ถ์ ๋ถ์ฌํ  ์ ์์ต๋๋ค. Eureka Client๋ง๋ค ์ด๋ฅผ ์ค์ ํด์ฃผ๋ฉด ๋ฉ๋๋ค.

๋จผ์ , [zones and regions](https://docs.spring.io/spring-cloud-netflix/docs/3.1.1/reference/html/#spring-cloud-eureka-server-zones-and-regions)
๋ฌธ์๋ฅผ ํตํด, ๊ฐ๊ฐ์ Zone๊ณผ Peer๋ฅผ ๊ตฌ์ฑํด์ฃผ์ด์ผ ํฉ๋๋ค. ์์ ๋งํฌ๋ฅผ ์ฐธ๊ณ ํ์ธ์.

๋ค์์ผ๋ก, ์๋น์ค๊ฐ ์ํด์์ด์ผ ํ  Zone์ ์ง์ ํด์ค์ผ ํฉ๋๋ค. metadataMap ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ์ฌ ์ค์ ํ๋ฉด ๋๋๋ฐ, ๋ง์ฝ service1์ด zone1๊ณผ zone2์ ๋ชจ๋
๋ฐฐํฌ๋์ด ์๋ค๋ฉด, ๊ฐ zone์ ์ํด ์๋ค๊ณ  ์ค์ ํด์ฃผ์ด์ผ ํฉ๋๋ค.

์๋์ ์์๋ฅผ ํ์ธํ์ธ์.

Service 1 in Zone 1

```java
eureka.instance.metadataMap.zone=zone1
    eureka.client.preferSameZoneEureka=true
```

Service 1 in Zone 2

```java
eureka.instance.metadataMap.zone=zone2
    eureka.client.preferSameZoneEureka=true
```

## ๐ Refreshing Eureka Clients

๊ธฐ๋ณธ์ ์ผ๋ก, `EurekaClient` ๋น์ ์๋ก๊ณ ์นจ์ด ๊ฐ๋ฅํฉ๋๋ค. ์ฆ, Eureka Client์ Properties๋ฅผ ์์ ํ๊ณ  ์๋ก๊ณ ์นจํ  ์ ์๋ค๋ ๋ป์๋๋ค. ์๋ก๊ณ ์นจ์ด ๋ฐ์ํ
Client๋ Eureka Server๋ก๋ถํฐ ๋ฑ๋ก์ด ํด์ ๋๊ณ , ์ฃผ์ด์ง ์๋น์ค์ ๋ชจ๋  ์ธ์คํด๋ฅผ ์ผ์  ์๊ฐ ์ฌ์ฉํ  ์ ์๊ฒ ๋ฉ๋๋ค. ์ด๋ฅผ ๋ฐฉ์งํ๊ธฐ ์ํด์, refresh ์ค์ ์
๋นํ์ฑํํ  ์๋ ์์ต๋๋ค.

```java
eureka.client.refresh.enable=false
```

## ๐ ****Using Eureka with Spring Cloud LoadBalancer****

Spring Cloud LoadBalancer `ZonePreferenceServiceInstanceListSupplier` ๋ฅผ ์ฌ์ฉํ  ์ ์์ต๋๋ค. Eureka instance
metadata(`eureka.instance.metadataMap.zone`)์ `zone`์ Instance ๋ณ๋ก Service Instance๋ฅผ ํํฐ๋งํ๋๋ฐ ์ฌ์ฉ๋๋
spring-cloud-loadbalancer-zone ์์ฑ ๊ฐ์ ์ค์ ํ๋๋ฐ ์ฌ์ฉ๋ฉ๋๋ค.

์์ ๋ด์ฉ์ ์ค์ ํ์ง ์๊ณ  `spring.cloud.loadbalancer.eureka.approximateZoneFromHostname`์ ๊ฐ์ `true`๋ก ์ค์ ํ  ๊ฒฝ์ฐ, ์๋ฒ
ํธ์คํธ ์ด๋ฆ์ ๋๋ฉ์ธ ์ด๋ฆ์ zone์ ๋ํ proxy๋ก ์ฌ์ฉํ  ์ ์์ต๋๋ค.

Zone ๋ฐ์ดํฐ์ ๋ค๋ฅธ ์์ค๊ฐ ์์ผ๋ฉด Client ๊ตฌ์ฑ(Instance๊ฐ ์๋)์ ๊ธฐ๋ฐ์ผ๋ก ์ถ์ธก๋์ด์ง๋๋ค. region ์ด๋ฆ์์ zone ๋ชฉ๋ก๊น์ง์
๋งต์ธ `eureka.client.availabilityZones`๋ฅผ ๊ฐ์ ธ ์์ Instance์ ์์ฒด region์ ์ฒซ ๋ฒ์งธ ์์ญ(์ฆ, `eureka.client.region`์
๊ธฐ๋ณธ Netflix์์ ํธํ์ฑ์ ์ํด "us-east-1"๋ก ๊ธฐ๋ณธ ์ค์ ๋จ)์ ๊ฐ์ ธ ์ต๋๋ค.
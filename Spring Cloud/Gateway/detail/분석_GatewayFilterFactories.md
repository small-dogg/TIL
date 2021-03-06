# GatewayFilter Factories

---

### ๐ **The Add**RequestHeader GatewayFilter Factory

๋ค์ด์คํธ๋ฆผ์ผ๋ก ์์ฒญ์ ๋ณด๋ด๊ธฐ ์ด์ ์, ์๋ก์ด ํค๋๋ฅผ ์์ฑํ์ฌ ๋ณด๋๋๋ค.

```yaml
routes:
- id: add_request_header_route
  uri: http://example.org
  filters:
  - AddRequestHeader=X-Request-red, blue
```

์ถ๊ฐ๋ก, AddRequestHeader Filter๋ URI ๋ณ์๋ฅผ ํ์ฉํ  ์๋ ์์ต๋๋ค.

```yaml
routes:
- id: add_request_header_route
  uri: http://example.org
  predicates:
  - Path=/red/**{segment}**
  filters:
  - AddRequestHeader=X-Request-red, blue-**{segment}**
```

### ๐ **The AddRequestParameter GatewayFilter Factory**

๋ค์ด์คํธ๋ฆผ์ผ๋ก ์์ฒญ์ ๋ณด๋ด๊ธฐ ์ด์ ์, ์๋ก์ด ํ๋ผ๋ฏธํฐ๋ฅผ ์์ฑํ์ฌ ๋ณด๋๋๋ค. ์์ฑํ  ๋๋ name๊ณผ value๋ฅผ ๊ตฌ๋ถํ์ฌ ํ๋ผ๋ฏธํฐ๋ฅผ ์์ฑํฉ๋๋ค.

```yaml
routes:
- id: add_request_parameter_route
  uri: http://example.org
  filters:
  - AddRequestParameter=red, blue
```

๋ง์ฐฌ๊ฐ์ง๋ก, URI ๋ณ์๋ฅผ ํ์ฉํ  ์ ์์ต๋๋ค.

```yaml
routes:
- id: add_request_parameter_route
  uri: http://example.org
  predicates:
  - Host={segment}.myhost.org
  filters:
  - AddRequestParameter=foo, bar-{segment}
```

### ๐ **The AddResponseHeader GatewayFilter Factory**

๋ค์ด์คํธ๋ฆผ์ ์๋ต ํค๋์ ์๋ก์ด ํค๋๋ฅผ ์ถ๊ฐํ  ์ ์์ต๋๋ค.

```yaml
routes:
- id: add_response_header_route
  uri: http://example.org
  filters:
  - AddResponseHeader=X-Request-red, blue
```

๋ง์ฐฌ๊ฐ์ง๋ก URI ๋ณ์๋ฅผ ํ์ฉํ  ์ ์์ต๋๋ค.

```yaml
routes:
- id: add_response_header_route
  uri: http://example.org
  predicates:
  - Path=/red/**{segment}**
  filters:
  - AddResponseHeader=X-Request-red, blue-**{segment}**
```

### ๐ **The DedupeResponseHeader GatewayFilter Factory**

์๋ต ํค๋์ ์ค๋ณต์ ์ ๊ฑฐํฉ๋๋ค.

```yaml
routes:
  - id: dedupe_response_header_route
    uri: https://example.org
    filters:
    - DedupeResponseHeader=Access-Control-Allow-Credentials Access-Control-Allow-Origin
```

CORS๋๋ฌธ์? ์๋ตํค๋ ์ค๋ณต์ ๊ฑฐ์ธ๊ฑด ์๊ฒ ๋๋ฐ CORS์ด์ผ๊ธฐ๋ ์ ๊ณ์ ๋์ค๋๊ฑฐ์ง....

### ๐ **The Spring Cloud CircuitBreaker GatewayFilter Factory**

Spring Cloud CircuitBreaker API๋ฅผ ์ฌ์ฉํ์ฌ, CircuitBreaker๋ฅผ Wrapping ํ๋ Filter์๋๋ค. Resilience4J๋ฅผ ์ฌ์ฉํฉ๋๋ค.(CirtcuitBreaker๋ฅผ ํ์ตํ๋ฉด ์ข๋ ๋ณด์ถฉ...)

```yaml
routes:
  - id: circuitbreaker_route
    uri: https://example.org
    filters:
      - CircuitBreaker=myCircuitBreaker
```

### ๐ **The FallbackHeaders GatewayFilter Factory**

### ๐ **The MapRequestHeader GatewayFilter Factory**

### ๐ **The PrefixPath GatewayFilter Factory**

### ๐ **The PreserveHostHeader GatewayFilter Factory**

### ๐ **The RequestRateLimiter GatewayFilter Factory**

### ๐ **The RedirectTo GatewayFilter Factory**

### ๐ **The RemoveRequestHeader GatewayFilter Factory**

### ๐ **The RemoveResponseHeader GatewayFilter Factory**

### ๐ **The RemoveResponseParameter GatewayFilter Factory**

### ๐ **The RewritePath GatewayFilter Factory**

### ๐ **The RewriteLocationResponseHeader GatewayFilter Factory**

### ๐ **The RewriteResponseHeader GatewayFilter Factory**

### ๐ **The SetPath GatewayFilter Factory**

### ๐ **The SetResponseHeader GatewayFilter Factory**

### ๐ **The SetRequestHostHeader GatewayFilter Factory**

### ๐ **The SaveSession GatewayFilter Factory**

### ๐ **The SecureHeaders GatewayFilter Factory**

### ๐ **The StripPrefix GatewayFilter Factory**

### ๐ **The Retry GatewayFilter Factory**

### ๐ **The RequestSize GatewayFilter Factory**

### ๐ Modify a Request Body **GatewayFilter Factory**

### ๐ Modify a Response Body **GatewayFilter Factory**

### ๐ Token Relay **GatewayFilter Factory**

### ๐ Default Filters
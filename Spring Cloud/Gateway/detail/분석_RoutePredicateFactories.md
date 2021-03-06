# Route Predicate Factories

### ๐ **The After Route Predicate Factory**

์์ฑ๋ datetime ์ดํ์ ๋ฐ์ํ ์์ฒญ๋ง์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค.  ์ฌ์ฉ๋๋ ํ๋ผ๋ฏธํฐ๋ datetime ์๋๋ค. ์ง์ ๋ datetime๋ณด๋ค ์ดํ์ ๋ฐ์ํ ์์ฒญ๋ง์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค.

```yaml
#์๋ต
predicates:
- After=2017-01-20T17:42:47.789-07:00[America/Denver]
```

### ๐ **The Before Route Predicate Factory**

After ์ ๋ฐ๋๋ก ์์ฑ๋ datetime ์ด์ ์ ๋ฐ์ํ ์์ฒญ๋ง์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค.

```yaml
predicates:
- Before=2017-01-20T17:42:47.789-07:00[America/Denver]
```

### ๐ **The Between Route Predicate Factory**

์ด๋ฆ์ฒ๋ผ ์ง์ ๋ datetime ์ฌ์ด์ ๋ฐ์ํ ์์ฒญ๋ง์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค. ์ฌ์ด์ ๊ฐ์ด ๋์์ด๊ธฐ ๋๋ฌธ์ ํ์ํ ํ๋ผ๋ฏธํฐ๋ 2๊ฐ์๋๋ค. After์ Before. ๋ฐ๋์ ๊ฐ์ ์๋ ฅํ ๋ ์ฒซ๋ฒ์งธ ์ธ์๋ After์ ํด๋นํ๋ ๊ฐ์ด์ด์ผํ๊ณ , ๋๋ฒ์งธ ์ธ์๋ Before์ ํด๋นํ๋ ๊ฐ์ด์ด์ผํฉ๋๋ค.

```yaml
predicates:
- Between=2017-01-20T17:42:47.789-07:00[America/Denver], 2017-01-21T17:42:47.789-07:00[America/Denver]
```

### ๐ **The Cookie Route Predicate Factory**

ํน์  Cookie์ ๊ฐ์ด value๋ก ์์ฑํ๋ ์ ๊ท์๊ณผ ์ผ์นํ  ๋ ์์ฒญ์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค. ๋์ Cookie์ ๊ทธ Cookie์ ๊ฐ์ด ๋์์ด๊ธฐ ๋๋ฌธ์ ํ์ํ ํ๋ผ๋ฏธํฐ๋ 2๊ฐ์๋๋ค. Cookie์ ์ด๋ฆ๊ณผ ์๋ฐ ์ ๊ท ํํ์. ์ฒซ๋ฒ์งธ ํ๋ผ๋ฏธํฐ๋ ์ด๋ฆ์ด์ด์ผํ๊ณ , ๋๋ฒ์จฐ ์ธ์๋ ์๋ฐ ์ ๊ท ํํ์์ด์ด์ผํฉ๋๋ค.

```yaml
predicates:
- Cookie=chocolate, ch.p
```

### ๐ **The Header Route Predicate Factory**

HTTP ์์ฒญ์ ํน์  Header์ ๊ทธ Header์ ๊ฐ์ด ์ผ์นํ  ๋ ์์ฒญ์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค. ๋ง์ฐฌ๊ฐ์ง๋ก Header Predicate๋ ํ๋ผ๋ฏธํฐ๊ฐ 2๊ฐ์๋๋ค. Header์ ์ด๋ฆ๊ณผ ์๋ฐ ์ ๊ทํํ์. ์ฒซ๋ฒ์งธ ํ๋ผ๋ฏธํฐ๋ ์ด๋ฆ์ด์ด์ผํ๊ณ , ๋๋ฒ์จฐ ์ธ์๋ ์๋ฐ ์ ๊ท ํํ์์ด์ด์ผํฉ๋๋ค.

```yaml
predicates:
- Header=X-Request-Id, \d+
```

### ๐ **The Host Route Predicate Factory**

HTTP ์์ฒญํ ๋์์ Host์ ๊ฐ์ด ์ผ์นํ  ๋ ์์ฒญ์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค. ํ๋ผ๋ฏธํฐ๋ก๋ Host์ URI๋ฅผ ๋์ดํ๋๋ฐ,  Ant-style ํจํด์ผ๋ก  **,**   ๋ก Host ์ ๋ณด๋ฅผ ๋์ดํฉ๋๋ค.

```yaml
predicates:
- Host=**.a.co.kr,**b.co.kr
```

์ถ๊ฐ๋ก, Host ์ ๋ณด๋ฅผ URI ํํ๋ฆฟ์ผ๋ก ๋ณ์ํํ์ฌ, ๊ฐ์ ์ถ์ถํ๊ณ  ์ฌ์ฉํ  ์๋ ์์ต๋๋ค.

์ด๋ฅผํ๋ฉด, {sub},a.co.kr ๋ก Host์ ๋ณด๋ฅผ ์์ฑํ์์ผ๋ฉด, **ServerWebExchange.getAttributes()**๋ก๋ถํฐ **URI_TEMPLATE_VARIABLES_ATTRIBUTE**์ ์ ์๋ ํค๋ก ๊ฐ์ ์ถ๊ฐํ์ฌ ์์ฑํ  ์ ์์ต๋๋ค. ๊ทธ๋ฌ๋ฉด Filter์์๋ ๋ณ์ ๊ฐ์ ํ์ฉํ  ์ ์๊ฒ ๋ฉ๋๋ค.

### ๐ **The Method Route Predicate Factory**

์์ฒญ์ HTTP Method ๋์์ด ์ผ์นํ  ๋ ์์ฒญ์ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค. ํ๋ผ๋ฏธํฐ๋ก๋ HTTP Method๋ฅผ ์์ฑํฉ๋๋ค.

```yaml
predicates:
- Method=GET,POST
```

### ๐ **The Path Route Predicate Factory**

Path Predicate๋ ์์ฒญ์ Path๊ฐ ์ผ์นํ  ๋ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค. ์คํ๋ง์ PathMatcher์ ๋ฆฌ์คํธ Patterns๋ฅผ ์ฌ์ฉํ๋ฉฐ, matchTrailingSlash๋ผ๋ ํ๋๊ทธ๊ฐ์ ์ ๊ณตํ์ฌ,  /  ๋ก ๋๋๋ Path๋ฅผ ํ์ฉํ ์ง์ ๋ํ ์ฌ๋ถ๋ฅผ ์์ฌ๊ฒฐ์ ํ  ์ ์์ต๋๋ค.

```yaml
predicates:
- Path:/a/{segment},/b/{segment}
matchTrailingSlash=false
```

/a/apple, /b/banana ์ ๊ฐ์ Path๋ ๋ชจ๋ ์ผ์นํฉ๋๋ค. ๋ค๋ง, /a/apple/์ ๊ฐ์ Path๋ matchTrailingSlash๊ฐ false์ด๊ธฐ ๋๋ฌธ์ ์ผ์นํ์ง ์๋ค๊ณ  ํ๋จํ์ฌ, ์์ฒญ์ด ๋งค์นญ๋์ง ์์ต๋๋ค.

### ๐ **The Query Route Predicate Factory**

HTTP ์์ฒญ ์ Query์ ์์ฑ๋ ํค์ ํด๋น ํค์ ๊ฐ์ด ํํ์๊ณผ ์ผ์นํ  ๋ ๋งค์นญ์์ผ์ฃผ๋ Predicate์๋๋ค. ํค๊ฐ๊ณผ ์๋ฐ ์ ๊ทํํ์. ์ฒซ๋ฒ์งธ ํ๋ผ๋ฏธํฐ๋ ํค์ ๊ฐ์ด์ด์ผํ๊ณ , ๋๋ฒ์จฐ ์ธ์๋ ์๋ฐ ์ ๊ท ํํ์์ด์ด์ผํฉ๋๋ค.

```yaml
predicates:
- Query=red, app.
```

red๋ผ๋ ํค์ ๊ฐ์ด apple์ด๊ฑฐ๋ apples์ ๊ฐ์ด app์ด ์ ๋ฐฉ์ ํฌํจ๋์ด์์ผ๋ฉด ์ผ์นํฉ๋๋ค.

### ๐ **The RemoteAddr Route Predicate Factory**

ํ๋ผ๋ฏธํฐ๋ CIDR ํ๊ธฐ๋ก ์์ฑํฉ๋๋ค. ์์ฒญ๋ ๋์์ RemoteAddr ๊ฐ์ ๋ฐ๋ผ CIDR ๋ด ํฌํจ๋๋ฉด ์ผ์นํฉ๋๋ค.

```yaml
predicates:
- RemoteAddr=192.168.1.1/24
```

192.168.1.1์ 24bit ๋ด ๋์ญ์ ์กด์ฌํ๋ ์์ฒญ์ RemoteAddr์ ๋ํด ์ผ์นํฉ๋๋ค.

### ๐ **The Weight Route Predicate Factory**

Weight Predicate๋ ๊ทธ๋ฃน์ ๋ถ๋ฆฌํ๊ณ  ๊ฐ์ค์น๋ฅผ ์์ฑํ์ฌ, ํธ๋ํฝ์ ์ ๋์ ๋ฐ๋ผ ๋์ ๊ทธ๋ฃน์ผ๋ก ์์ฒญ์ ์ ๋ฌํ๋ Predicate์๋๋ค. group๊ณผ weight ํ๋ผ๋ฏธํฐ๋ฅผ ์ฌ์ฉํฉ๋๋ค.

```yaml
routes:
- id: weight_high
  uri: https://weighthigh.org
  predicates:
  - Weight=group1, 8
- id: weight_low
  uri: https://weightlow.org
  predicates:
  - Weight=group1, 2
```

80%์ ์์ฒญ์ weigh_high๋ก ์์ฒญ์ด ์ ๋ฌ๋๊ณ , 20%์ ์์ฒญ์ weigh_low๋ก ์ ๋ฌ๋ฉ๋๋ค.
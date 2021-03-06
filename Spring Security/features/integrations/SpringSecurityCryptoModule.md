# Spring Security Crypto Module

## ๐ Introduction

Spring Security Crypto module์ ๋์นญ ์ํธํ ๋ฐ ํค ์์ฑ, ์ํธ ์ธ์ฝ๋ฉ์ ์ง์ํฉ๋๋ค.  ์ฝ๋๋ ์ฝ์ด ๋ชจ๋๊ณผ ํจ๊ป ๋ฐฐํฌ๋์ง๋ง, ๊ทธ ์ด๋ค Spring Security ์ฝ๋์์ ์์กด์ฑ์ ๊ฐ๊ณ  ์์ง ์์ต๋๋ค.

## ๐ Encryptors

Encryptors ํด๋์ค๋ ๋์นญ ์ํธํ๋ฅผ ์ํ symmetric encryptor๋ฅผ ์์ฑํ๋ ํฉํ ๋ฆฌ ๋ฉ์๋๋ฅผ ์ ๊ณตํฉ๋๋ค. ํด๋์ค๋ฅผ ์ฌ์ฉํ๋ฉด, raw byte[] ํํ๋ก ์ํธํํ๋ ByteEncryptors๋ฅผ ๋ง๋ค ์ ์์ต๋๋ค. ๋ํ text string์ ์ํธํํ๋ TextEncryptors๋ฅผ ๋ง๋ค ์๋ ์์ต๋๋ค. ์ด Encrytor๋ Thread-safe ํฉ๋๋ค.

### BytesEncryptor

`Encryptors.stronger` ํฉํ ๋ฆฌ ๋ฉ์๋๋ฅผ ์ฌ์ฉํ์ฌ BytesEncryptor๋ฅผ ์์ฑํฉ๋๋ค:

**Bytes Encryptor**

```java
Encryptors.stronger("password", "salt");
```

โstrongโ ์ํธํ ๋ฉ์๋๋ 256 bit ์ํธ ๊ฐ๋์ AES ์ํธํ ๋ฐฉ์์ด๋ฉฐ, Galois Counter Mode (GCM) encryptor๋ฅผ ์์ฑํฉ๋๋ค. ์ด๋ PKCS #5์ PBKDF2๋ฅผ ์ฌ์ฉํ์ฌ secret key๋ฅผ ์ป์ต๋๋ค. ์ด ๋ฉ์๋๋ Java 6๋ฒ์  ์ด์์ ์๊ตฌํฉ๋๋ค. ํจ์ค์๋๋ ๋น๋ฐํค๋ฅผ ๋ง๋ค ๋ ์ฌ์ฉ๋๋ฉฐ, ์ด๋ ์์ ํ ์ฅ์์ ๋ณด๊ดํด์ผํ๋ฉฐ ๊ณต์ ํด์  ์๋ฉ๋๋ค. salt๋ ์ํธํํ ๋ฐ์ดํฐ๊ฐ ์์๋์์ ๋ ํค์ ๋ํ ์ฌ์  ๊ณต๊ฒฉ์ ๋ฐฉ์งํ๊ธฐ ์ํด ์ฌ์ฉ๋ฉ๋๋ค. 16-byte ๋์ ์ด๊ธฐํ ๋ฐฑํฐ๋ ๊ฐ๊ฐ์ ์ํธํ๋ ๋ฉ์์ง์ ์ ์ฉ๋๋ฉฐ, ์ด๋ ์ ๋ํฌ ํฉ๋๋ค.

์ ๊ณต๋๋ salt๋ hex๋ก ์ธ์ฝ๋ฉ ๋ String ํํ์ด์ด์ผ ํ๋ฉฐ, ๋์์ด๋ฉฐ, ์ต์ 8byte ์ด์์ ๊ธธ์ด์ด์ผํฉ๋๋ค. ์๋๋ KeyGenerator๋ฅผ ์ฌ์ฉํ์ฌ salt๋ฅผ ๋ง๋๋ ๋ฐฉ๋ฒ์ ์์์๋๋ค:

```java
String salt = KeyGenerators.string().generateKey(); // generates a random 8-byte salt that is then hex-encoded
```

์ฌ์ฉ์๋ ๋ํ 256-bit ๊ฐ๋์ AES ์ํธํ, CBC ๋ชจ๋์ ํด๋นํ๋ `standard` ์ํธํ ๋ฉ์๋๋ฅผ ์ฌ์ฉํ  ์ ์์ต๋๋ค. ์ด ๋ชจ๋๋ [์ธ์ฆ](https://en.wikipedia.org/wiki/Authenticated_encryption) ๋์ง ์์์ผ๋ฉฐ ๋ฐ์ดํฐ์ ์ง์ ์ฌ๋ถ๋ฅผ ๋ณด์ฅํ์ง ์์ต๋๋ค. ๊ฐ๊ธ์ ์ด๋ฉด, `Encryptors.stronger` ๋ฅผ ์ฌ์ฉํ๋ ๊ฒ์ด ์ข์ต๋๋ค.

### TextEncryptor

`Encryptors.text` ํฉํ ๋ฆฌ ๋ฉ์๋๋ฅผ ์ฌ์ฉํ์ฌ TextEncryptor๋ฅผ ์์ฑํฉ๋๋ค:

**TextEncryptor**

```java
Encryptors.text("password", "salt");
```

TextEncryptor๋ ํ์ค BytesEncryptor๋ฅผ ์ฌ์ฉํ์ฌ text ๋ฐ์ดํฐ๋ฅผ ์ํธํํฉ๋๋ค. ์ํธํ๋ ๊ฒฐ๊ณผ๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค ๋๋ ํ์ผ์์คํ์ ์ฝ๊ฒ ์ ์ฅํ  ์ ์๋๋ก hex ์ธ์ฝ๋ฉ๋ String ๊ฐ์ผ๋ก ๋ฐํ๋ฉ๋๋ค.

โqueryableโํ TExtEncryptor๋ฅผ ์์ฑํ  ๋ Encryptors.queryableText ํฉํฐ๋ฆฌ ๋ฉ์๋๋ฅผ ์ฌ์ฉํฉ๋๋ค.

```java
Encryptors.queryableText("password","salt");
```

queryable TextEncryptor์ standard TextEncryptor์ ์ฐจ์ด์ ์ initialization vector(IV) ๋ฅผ ๋ค๋ฃจ๋์ง์ ๋ํ ์ฌ๋ถ์๋๋ค. IV๋ queryable TextEncryptor#encrypt ์ฐ์ฐ์์ ์ฌ์ฉํ๋๋ฐ, ์ด๋ฅผ ๊ณต์ ํ๊ฑฐ๋, ๊ณ ์ ๋ ๊ฐ์ด๋ฉฐ, ๋ฌด์์๋ก ์์ฑํ์ง ์์ต๋๋ค. ๊ทธ ๋ง์ธ์ฆ์จ, ๋์ผํ ํ์คํธ๋ฅผ ์ฌ๋ฌ ๋ฒ ์ํธํํ์ ๋, ๋งค๋ฒ ๋์ผํ ์ํธํ๋ ๊ฒฐ๊ณผ๋ฅผ ์ค๋ค๋ ์๋ฏธ์๋๋ค. ์ด๋ ๋ ์์ ํ์ง๋ง, ์ํธํ ๋ฐ์ดํฐ๋ฅผ ์ง์ํด์ผํ  ๋๋ ํ์ํฉ๋๋ค. OAuth API KEY ๊ฐ์ ๊ฒฝ์ฐ ์ง์๊ฐ๋ฅํ ์ํธํ๋ ํ์คํธ๋ฅผ ์ฌ์ฉํฉ๋๋ค.

## ๐ Key Generators

KeyGenerators ํด๋์ค๋ ์๋ก ๋ค๋ฅธ ํ์์ key generator๋ฅผ ์์ฑํ๋๋ฐ ์ ์ฉํ ํฉํฐ๋ฆฌ ๋ฉ์๋ ๋ช๊ฐ์ง๋ฅผ ์ ๊ณตํฉ๋๋ค. ์ด ํด๋์ค๋ฅผ ์ฌ์ฉํ๋ฉด, byte[] key๋ฅผ ์์ฑํ๋ `BytesKeyGenerator`, string ํํ์ key๋ฅผ ์์ฑํ๋ `StringKeyGenerator` ๋ฅผ ์์ฑํ  ์ ์์ต๋๋ค. Key Generators๋ thread-safe ํฉ๋๋ค.

### BytesKeyGenerator

SecureRandom ์ธ์คํด์ค๋ฅผ ๋ท๋ฐ์นจํ๋ BytesKeyGenerator๋ฅผ ์์ฑํ  ๋์๋ BytesKeyGenerator KeyGenerators.secureRandom ํฉํ ๋ฆฌ ๋ฉ์๋๋ฅผ ์ฌ์ฉํฉ๋๋ค:

**BytesKeyGenerator**

```java
BytesKeyGenerator generator = KeyGenerators.secureRandom();
byte[] key = generator.generateKey();
```

๊ธฐ๋ณธ ํค ๊ธธ์ด๋ 8bytes ์๋๋ค. ์ด๋ ๋ํ `KeyGenerators.secureRandom` ๋ฉ์๋์ ํค ๊ธธ์ด๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ์์ฑํ์ฌ, ํค ๊ธธ์ด๋ฅผ ๋ณ๊ฒฝํ  ์๋ ์์ต๋๋ค.

```java
KeyGenerators.secureRandom(16);
```

ํญ์ ๋์ผํ ํค๋ฅผ ๋ฆฌํดํ๋ BytesKeyGenerator๋ฅผ ๋ง๋ค๋๋ `KeyGenerators.shared` ํฉํฐ๋ฆฌ ๋ฉ์๋๋ฅผ ์ฌ์ฉํฉ๋๋ค.

```java
KeyGenerators.shared(16);
```

### StringKeyGenerator

๊ฐ ํค๋ฅผ hex-์ธ์ฝ๋ฉํ๋ 8 bytes SecureRandom KeyGenerator๋ฅผ ๋ง๋ค ๋๋ `KeyGenerators.string` ํฉํ ๋ฆฌ ๋ฉ์๋๋ฅผ ์ฌ์ฉํฉ๋๋ค:

```java
KeyGenerators.string();
```

## ๐ Password Encoding

spring-security-crypto ๋ชจ๋์ password ํจํค์ง๋ ํจ์ค์๋ ์ธ์ฝ๋ฉ ์ง์์ ์ ๊ณตํฉ๋๋ค. `PasswordEncoder` ๋ ํต์ฌ ์๋น์ค ์ธํฐํ์ด์ค์ด๋ฉฐ ์๊ทธ๋์ฒ๋ ์๋์ ๊ฐ์ต๋๋ค.

```java
public interface PasswordEncoder {
	String encode(String rawPassword);
	boolean matches(String rawPassword, String encodedPassword);
}
```

`matches` ๋ฉ์๋๋ rawPassword์ ์ํธํ๋ Password์ ๋น๊ต ๊ฒฐ๊ณผ๊ฐ ์ผ์นํ๋์ง๋ ๋ฐํํด์ค๋๋ค. ์ด ๋ฉ์๋๋ Password-based ์ธ์ฆ ์คํค๋ง๋ฅผ ์ง์ํ๊ธฐ ์ํด ๋ง๋ค์ด์ก์ต๋๋ค.

`BCryptPasswordEncoder` ๊ตฌํ์ฒด๋ โbcryptโ๋ฅผ ๊ด๋ฒ์ํ๊ฒ ์ง์ํ๋ ๋น๋ฐ๋ฒํธ ํด์ ์๊ณ ๋ฆฌ์ฆ์๋๋ค.

Bcrypt๋ 16 byte ๊ธธ์ด์ ๋๋คํ salt ๊ฐ์ ์ฌ์ฉํ๋ฉฐ, Password cracker๋ก๋ถํฐ cracking(i.e. BRute Force Attack, Rainbow Table Attack ๋ฑ ํจ์ค์๋๋ฅผ ์์๋ด๊ธฐ ์ํด ๊ณต๊ฒฉํ๋ ๊ฒ์ ์ผ์ปฌ์)ํ  ์ ์๋๋ก ์๋์ ์ผ๋ก ๋๋ฆฌ๊ฒ ๋์ํฉ๋๋ค. ์์๋์ โstrengthโ ํ๋ผ๋ฏธํฐ๋ฅผ ์กฐ์ ํ์ฌ 4~31 ๊ฐ์ผ๋ก ํ๋ํ  ์ ์์ต๋๋ค. โstrengthโ์ ๊ฐ์ด ๋์์๋ก, ํด์ ๊ฐ์ ๊ณ์ฐํ๊ธฐ ์ํด ๋ ๋ง์ ์์ ์์์ ์ํํฉ๋๋ค. ๊ธฐ๋ณธ๊ฐ์ 10์๋๋ค. ์ด ๊ฐ์ ์ธ์ฝ๋ฉํ ํด์์ ํจ๊ผ ์ ์ฅ๋๋ฏ๋ก ๊ธฐ์กด์ ๋ฐฐํฌํ ์์คํ์์๋ ๋น๋ฐ๋ฒํธ์ ์ํฅ ์์ด ์ด ๊ฐ์ ๋ณ๊ฒฝํ  ์ ์์ต๋๋ค.

```java
// Create an encoder with strength 16
BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(16);
String result = encoder.encode("myPassword");
assertTrue(encoder.matches("myPassword", result));
```

`Pbkdf2PasswordEncoder` ๊ตฌํ์ฒด๋ PBKDF2 ์๊ณ ๋ฆฌ์ฆ์ผ๋ก ๋น๋ฐ๋ฒํธ๋ฅผ ํด์ฑํ  ๋ ์ฌ์ฉํฉ๋๋ค. PBKDF2๋ Password cracker๋ก๋ถํฐ cracking ํ  ์ ์๋๋ก ์๋์ ์ผ๋ก ๋๋ฆฌ๊ฒ ๋์ํฉ๋๋ค.์์คํ์์ ๋น๋ฐ๋ฒํธ ํ๋๋ฅผ ๊ฒ์ฆํ๋ ๋ฐ์๋ 0.5์ด ์ ๋๊ฐ ์์๋๋๋ก ์ค์ ํด์ผ ํฉ๋๋ค.
# Сервис для работы с пользователями 🤟

> **Для работы с сервисом необходимо подключения к инфраструктуре колледжа.**

## Пример работы с сервисом на Spring boot

### Добавляем зависимости для работы с gRPC в Spring boot.

Это необходимо потому что логика общения между сервисами настроенна через gRPC.

```gradle
// Файл build.gradle

plugins {
  // Подключаем плагин для интеграции gRPC с Spring Boot
  id("io.github.lognet.grpc-spring-boot") version "5.1.5"
}

// Применяем плагин gRPC Spring Boot
apply(plugin = "io.github.lognet.grpc-spring-boot")

buildscript {
  // Определяем репозитории для поиска зависимостей
  repositories {
    // Используем репозиторий плагинов Gradle
    maven {
      url = uri("https://plugins.gradle.org/m2/")
    }
  }
  dependencies {
    // Добавляем зависимость для использования gRPC Spring Boot Starter Gradle Plugin
    classpath("io.github.lognet:grpc-spring-boot-starter-gradle-plugin:5.1.5")
  }
}
```

### Добавление protobuf файла

Создайте папку proto в директории вашего проекта.

```
src/
  main/ # <--- Здесь создайте папку.
    java/...
    proto/ # <--- Вот так.
    resources/...
```

После создания папки перенесите protobuf файл в эту папку, и запустите проект.

### Отправка запроса на сервер

Для отправки запроса на наш сервер, вам необходимо создать функцию которая будет отправлять запрос на сервер.

Вот пример в качестве написанного сервиса:

```java
package ru.example.service;

// ...imports

@Service
public class AuthenticationClient {
  @Value("api.url")
  private String host;

  @Value("api.port")
  private Int host = 0;

  public String authenticate(String token) {
    ManagedChannel channel = ManagedChannelBuilder.forAddress(this.host, this.port).usePlaintext().build();
    AuthenticationServiceGrpc.AuthenticationServiceBlockingStub stub = AuthenticationServiceGrpc.newBlockingStub(channel);

    // Создаем тело запроса.
    JWTPayload payload = JWTPayload.newBuilder().setAccess(token).build();

    // Отправка запроса и получение пользователя.
    User current = stub.getUserByAccess(payload);

    return current.getUid();
  }
}
```

## Полезные ссылки:

- <b>Базовый url сервиса</b> `http`\
   http://10.3.24.109:8100/users/api/v1

- <b>Swagger</b> `http`\
  http://10.3.24.109:8100/users/api/v1/swagger-ui/index.html

- <b>Protobuf файл</b> `grpc`\
  https://github.com/ithub-announcement/w-authentication/releases

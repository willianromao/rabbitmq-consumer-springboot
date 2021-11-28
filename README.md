# rabbitmq-consumer-springboot
Aplicação do curso RabbitMQ para consumo de mensagens de uma fila.

## Variáveis de ambiente do application.yml

```yaml
server:
  port: ${CONSUMER_PORT:8082}
spring:
  rabbitmq:
    host: ${MQ_HOST:localhost}
    port: ${MQ_PORT:5672}
    username: ${MQ_USER:guest}
    password: ${MQ_PASS:guest}
```

## Exemplo de execução

```shell
docker build -t rabbitmq-consumer-springboot .
docker run -d -e MQ_HOST=192.168.0.209 rabbitmq-consumer-springboot
```

## Dockerfile
```dockerfile
FROM maven:3.3-jdk-8 AS build
WORKDIR /app
COPY . ./build/
WORKDIR /app/build/
RUN mvn clean install

FROM openjdk:8-jdk-alpine AS runtime
WORKDIR /app
ARG JAR_FILE=/app/build/target/*.jar
COPY --from=build ${JAR_FILE} /app/rabbitmq-consumer-springboot.jar
ENTRYPOINT ["java","-jar","/app/rabbitmq-consumer-springboot.jar"]
```

## Fonte
https://www.udemy.com/course/rabbitmq-com-springboot-e-docker/

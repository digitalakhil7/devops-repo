```bash
FROM ubuntu:18.04
RUN apt update -y && apt install openjdk-17-jdk -y
COPY spring-petclinic-3.1.0-SNAPSHOT.jar /spring-petclinic-3.1.0-SNAPSHOT.jar
EXPOSE 8080
CMD ["java","-jar","spring-petclinic-3.1.0-SNAPSHOT.jar"]
```

```bash
FROM maven:3.8.5-openjdk-17
COPY spring-petclinic-3.1.0-SNAPSHOT.jar /spring-petclinic-3.1.0-SNAPSHOT.jar
EXPOSE 8080
CMD ["java","-jar","spring-petclinic-3.1.0-SNAPSHOT.jar"]
```

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

### ARG, WORKDIR
```bash
FROM maven:3.8.5-openjdk-17
ARG SRC_DIR=/opt/spring
RUN mkdir -p $SRC_DIR
WORKDIR $SRC_DIR
COPY spring-petclinic-3.1.0-SNAPSHOT.jar $SRC_DIR/spring-petclinic-3.1.0-SNAPSHOT.jar
EXPOSE 8080
CMD ["java","-jar","spring-petclinic-3.1.0-SNAPSHOT.jar"]
```

### ENV
```bash
FROM ubuntu:18.04
ENV course="K8S Av"
ENV days="45days"
CMD echo "Welcome to $course for $days"
```

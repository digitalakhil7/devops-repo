### Gradle in sonar
Project: https://github.com/GoogleCloudPlatform/cloud-ops-sandbox/tree/develop

Pre-req: Java 11, dev branch and use adservice

Add plugin to `build.gradle`

### To Test
```bash
sh gradlew installDist -Dorg.gradle.java.home=/usr/lib/jvm/java-11-openjdk-11.0.19.0.7-1.el7_9.i386
```

### To Run Sonar
```bash
./gradlew sonarqube -Dsonar.projectKey=gcpgradle -Dsonar.host.url=http://34.125.107.34:9000 -Dsonar.login=squ_b44fc7c05b9ed8960f2175fe6bde2d7a730c941e -Dorg.gradle.java.home=/usr/lib/jvm/java-11-openjdk-11.0.19.0.7-1.el7_9.i386
```

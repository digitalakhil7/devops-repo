### 1. Pushing Jars (mvn deploy)

```bash
</dependencies>
```

```bash
<distributionManagement>
    <repository>
        <id>nexus</id>  
        <name>Release Repos</name>
        <url>http://34.125.150.243:8081/repository/first-release/</url>
     </repository>
    <snapshotRepository>
        <id>nexus</id>
        <name>Snapshot Repos</name>
        <url>http://34.125.150.243:8081/repository/first-snapshot/</url>
    </snapshotRepository>
</distributionManagement>
```

```bash
  <server>
    <id>nexus</id>
    <username>admin</username>
    <password>akhil</password>
  </server>
```
```bash
</servers>
```

### 2. Pulling Jars(mvn package)
```bash
<properties/>
```
```bash
<repositories>
    <repository>
        <id>nexus</id>
        <name>Akhil Remote Repo</name>
        <url>http://34.125.150.243:8081/repository/mixed/</url>
    </repository>
</repositories>
```

### remove old mirror**
```bash
<mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>external:http:*</mirrorOf>
      <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
      <url>http://34.125.150.243:8081/repository/mixed/</url>
    </mirror>
  </mirrors>
```

### 3. Pulling with proxy(delete junit jar in ~/.m2/repository/junint and mvn package)
Proxy (http://34.125.150.243:8081/repository/akhil-proxy/)

change url in repositories tag and mirror

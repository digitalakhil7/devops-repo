### Snapshot vs Release
same artifact can be redeployed in Snapshot repository
### Pushing vs Pulling
```table
                        Push                              Pull (Proxy/Group)
pom.xml                 distributonManagement             repositories
settings.xml (maven)    server                            mirror
```
### Proxy vs Group
Proxy - central dependency comes with proxy <br>
Group - cemtral dependency comes with proxy and custom dependency comes from RR <br>
![Nexus](https://github.com/digitalakhil7/devops-repo/assets/63640168/05d70c1f-36bd-46b7-bb64-a51675384ca1)


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

### 3. Pulling with Proxy(delete junit jar in ~/.m2/repository/junint and mvn package)
maven2 (proxy) <br>
name: akhil-proxy <br>
version policy: mixed <br>
proxy -> remote storage: copy same url shown <br>
**change url in repositories tag and mirror

Proxy (http://34.125.150.243:8081/repository/akhil-proxy/)

### 3. Pulling with Group(delete junit jar in ~/.m2/repository/junint and mvn package)
maven2 (group) <br>
name: group <br>
version policy: mixed <br>
member repositories: proxy, rr <br>
**change url in repositories tag and mirror

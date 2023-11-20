### Nexus Practise Pipeline
```bash
pipeline{
    agent any
    tools{
        maven 'maven3'
        jdk 'java8'
    }
    stages{
        stage('Download'){
            steps{
              git 'https://github.com/digitalakhil7/spring3-mvc-maven-xml-hello-world.git'
            }
        }
        stage('Build and Deploy'){
            steps{
                sh 'mvn clean deploy'
            }
        }
    }
}
```

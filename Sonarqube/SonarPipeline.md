### Requirements
* SonarQube Scanner (plugin)
* Manage Jenkins -> System
![sonarscannerconfig](https://github.com/digitalakhil7/devops-repo/assets/63640168/1eaadf2c-dfbc-40a3-a1e7-56ebcb8836de)

### Jenkins Pipeline
```bash
pipeline {
      agent{
          docker {image 'maven:3.8.6-openjdk-11'}
      }
  stages {
    stage('Download') {
      steps {
        git 'https://github.com/digitalakhil7/spring3-mvc-maven-xml-hello-world.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    // https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/jenkins-extension-sonarqube/#jenkins-pipeline
    stage('SonarQube Analysis') {
        steps {
            withSonarQubeEnv('mysonar') {
                sh 'mvn sonar:sonar'
            }
        }
    }
  }
}
```

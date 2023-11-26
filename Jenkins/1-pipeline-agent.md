## Agent
any
```bash
pipeline{
    agent any
    stages{
        stage('First'){
            steps{
                echo 'First'
            }
        }
    }
}
```
none
```bash
pipeline{
    agent none
    stages{
        stage('First'){
            agent{
                label 'java-slave'
            }
            steps{
                echo 'First'
            }
        }
    }
}
```
label
```bash
pipeline{
    agent{
        label 'java-slave'
    }
    stages{
        stage('First'){
            steps{
                echo 'First'
                sh 'hostname -i'
            }
        }
    }
}
```
node - not used in real time
```bash
pipeline{
    agent{
      node{
        label 'java-slave'
        customWorkspace '/home/akhil/custom23'
        }
    }
    stages{
        stage('First'){
            steps{
                echo 'First'
            }
        }
    }
}
```
#### 2 Agents
```bash
pipeline{
    agent{
        label 'java-slave'
    }
    stages{
        stage('First'){
            steps{
                echo 'First'
                sh 'hostname -i'
            }
        }
        stage('Second'){
            agent{
                label 'node-slave'
            }
            steps{
                echo 'Second'
                sh 'hostname -i'
            }
        }
    }
}
```
### Docker Agent - Download Build
```bash
pipeline {
      agent{
          docker {image 'maven:3.8.6-openjdk-8'}
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
stage('Deploy') {
      steps {
        deploy adapters: [tomcat9(credentialsId: 'tomcat_creds', path: '', url: 'http://34.170.246.173:30601/')], contextPath: '/myapp', onFailure: false, war: '**/*.war'
      }
    }
    
  }
}
```

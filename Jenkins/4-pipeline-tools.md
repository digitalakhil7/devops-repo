## Tools
### Maven
```bash
pipeline{
    agent any
    tools{
        maven '3.8.8'
    }
    stages{
        stage('build'){
            steps{
                sh 'mvn --version'
            }
        }
    }
}
```
### Maven with Multiple Java versions
```bash
pipeline{
    agent any
    tools{
        maven '3.8.8'
    }
    stages{
        stage('java 11'){
            steps{
                sh 'mvn --version'
            }
        }
        stage('java 17'){
            tools{
                jdk 'jdk17'
            }
            steps{
                sh 'mvn --version'
            }
        }
    }
}
```

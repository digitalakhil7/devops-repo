### Nexus Practise Pipeline - XML Config
distributonManagement tag in pom.xml and server tag is setting.xml (maven)
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
### Nexus Practise Pipeline - Using nexusArtifactUploader
Plugins: Nexus Artifact Uploader, Pipeline Utility Steps
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
    stage('Nexus'){
    steps{
        script{
            pom = readMavenPom file : "pom.xml"
            files = findFiles(glob: "target/*.${pom.packaging}")
            // if artifact exists
            if(fileExists (files[0].path)){
                echo "starting nexus upload"
            // https://plugins.jenkins.io/nexus-artifact-uploader/
                nexusArtifactUploader(
                    nexusVersion: "nexus3",
                    protocol: "http",
                    nexusUrl: "34.125.203.138:8081",
                    groupId: "${pom.groupId}",
                    // version: "${pom.version}",
                    version: "1.4",
                    repository: "nov26",
                    credentialsId: 'nexus_creds',
                    artifacts: [
                        [artifactId: "${pom.artifactId}",
                        classifier: '',
                        file: "${files[0].path}",
                        type: "${pom.packaging}"]
                    ]
                )
            }
            else{
                error "artifact does not exists"
            }
        }
    }
    }
  }
}
```

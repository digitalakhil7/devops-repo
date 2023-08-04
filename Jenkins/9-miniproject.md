## mini project - Git + Maven + Nexus + Tomcat
### (readMavenPom, findFiles) - Basic Steps, fileExists - Utility Steps
```bash
pipeline{
    agent any
    tools{
        maven '3.8.8'
    }
    environment{
        tomkey = credentials('tomcat_creds')
        NEXUS_VERSION = 'nexus3'
        NEXUS_PROTOCOL = 'http'
        NEXUS_URL = '34.16.167.146:8081'
        NEXUS_REPO = 'august-mix-repo'
    }
    stages{
        stage('Download'){
            steps{
              git 'https://github.com/digitalakhil7/spring3-mvc-maven-xml-hello-world.git'
            }
        }
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
                }
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
                            nexusVersion: "$env.NEXUS_VERSION",
                            protocol: "${env.NEXUS_PROTOCOL}",
                            nexusUrl: "${env.NEXUS_URL}",
                            groupId: "${pom.groupId}",
                            // version: "${pom.version}",
                            version: "${BUILD_NUMBER}",
                            repository: "${env.NEXUS_REPO}",
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
        stage('Deploy'){
            steps{
                //deploy adapters: [tomcat9(credentialsId: 'tomcat_creds', path: '', url: 'http://34.16.143.80:9090')], contextPath: '/julyjuly', onFailure: false, war: '**/*.war'
                //https://tomcat.apache.org/tomcat-8.5-doc/manager-howto.html
                //sh "curl -v -u ${tomkey_USR}:${tomkey_PSW} -T /var/lib/jenkins/workspace/Pipelines/p6/target/spring3-mvc-maven-xml-hello-world-1.5-SNAPSHOT.war 'http://34.16.143.80:9090/manager/text/deploy?path=/foi&update=true'"
                echo "deployed"
            }
        }
    }
}
```

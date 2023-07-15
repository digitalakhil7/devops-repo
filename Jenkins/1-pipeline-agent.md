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

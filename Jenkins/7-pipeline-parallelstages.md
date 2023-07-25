### parallel stages - Plugin (Blue Ocean)
**Note:** parallel stages generate longer mixed logs which causes ambiguity  - not recommended
```bash
pipeline{
    agent any
    stages{
        stage('Scans'){
            parallel{
                stage('One'){
                    steps{
                    sleep 10
                    echo "Stage One Completed"
                    }
                }
                stage('Two'){
                    steps{
                    sleep 15
                    echo "Stage Two Completed"
                    }
                }
                stage('Three'){
                    steps{
                    sleep 20
                    echo "Stage Three Completed"
                    }
                }
            }
        }
        stage('Deploy'){
            steps{
                echo "Deployment Done"
            }
        }
    }
}
```
### parallel stages - failFast true - if one step fails the next steps fail
failFast - not used in real time
```bash
pipeline{
    agent any
    stages{
        stage('Scans'){
            failFast true
            parallel{
                stage('One'){
                    steps{
                    sleep 10
                    echo "Stage One Completed"
                    }
                }
                stage('Two'){
                    steps{
                    sleep 15
                    echo "Stage Two Completed"
                    error "failed 404"
                    }
                }
                stage('Three'){
                    steps{
                    sleep 20
                    echo "Stage Three Completed"
                    }
                }
            }
        }
        stage('Deploy'){
            steps{
                echo "Deployment Done"
            }
        }
    }
}
```

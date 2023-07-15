## Workflow-basic-steps, Plugin - Pipeline: Basic Steps
Used in script{ } or steps{ }
### Link
```bash
https://www.jenkins.io/doc/pipeline/steps/workflow-basic-steps/
```
```bash
sleep - pause step
error - fail step
retry - only if step fails
timeout - if step takes long time
```

### sleep
```bash
pipeline{
    agent any
    stages{
        stage('Script'){
            steps{
                sleep 10
            }
        }
    }
}
```
### error and retry
```bash
pipeline{
    agent any
    stages{
        stage('Script'){
            steps{
                retry(3){
                    sleep 5
                    error 'some exception'
                }
                echo "retry success"
            }
        }
    }
}
```
### timeout
```bash
pipeline{
    agent any
    stages{
        stage('Script'){
            steps{
                timeout(time: 10, unit: 'SECONDS') {
                sleep 30
                }
            }
        }
    }
}
```


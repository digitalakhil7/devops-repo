## input
```bash
pipeline{
    agent any
    stages{
        stage('Deploy'){
            steps{
                timeout(time: 300, unit: 'SECONDS'){
                input message: "Deploy the App?", ok: "yes", submitter: "akhil,anand"
                }
                echo "Application Deployed"
            }
        }
    }
}
```
## input parameters
```bash
pipeline{
    agent any
    stages{
        stage('Deploy'){
            options{
                timeout(time: 30, unit: 'SECONDS')
            }
            input{
                message "Should we continue?"
                ok "Yes, we should."
                submitter "akhil"
                submitterParameter "whoapproved"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                    choice(name: 'CHOICE', choices: ['dev','test','prod'],description: 'where to deploy')
                }
            }
            steps{
                echo "Application Deployed by ${PERSON}"
                echo "Application approved by ${whoapproved}"
            }
        }
    }
}
```

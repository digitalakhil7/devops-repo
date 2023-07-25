## parameters
```bash
pipeline{
    agent any
    parameters{
         string(name: 'NAME', defaultValue: 'Akhil',description: 'Your Name')
         booleanParam(name: 'APPROVED', defaultValue: true, description: 'Toggle this value')
         choice(name: 'COUNTRIES', choices: ['India','Austrlia','USA'], description: 'select any country')
         credentials(name: 'CREDS',required: true, description: 'my creds')
    }
    stages{
        stage('Build'){
            steps{
                echo "Hello ${params.NAME}"
                echo "Approval: ${params.APPROVED}"
                echo "Country: ${params.COUNTRIES}"
                echo "Creds: ${params.CREDS}"
            }
        }
    }
}
```
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

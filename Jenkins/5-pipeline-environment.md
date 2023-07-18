## Environment - credentials( ), when - condition, allOf, anyOf
### 1. Global and Local Variable ( has more importance )
```bash
pipeline{
    agent any
    // Global variable
    environment{
        course = "K8S"
        name = "Global"
    }
    stages{
        stage('Course'){
            // Local variable has more importance
            environment{
                name = "Akhil"
            }
            steps{
                echo "Course is: ${course}"
                echo "Name is: ${name}"
            }
        }
    }
}
```
### 2. printenv - to print all env variables
```bash
pipeline{
    agent any
    stages{
        stage('Course'){
            steps{
                sh "printenv"
            }
        }
    }
}
```
### 3. credentials - git example
**_USR** - to get username

**_PSW** - to get password
```bash
pipeline{
    agent any
    environment{
        gitkey = credentials('github_creds')
    }
    stages{
        stage('Download'){
            steps{
                echo "Credentials: ${gitkey}"
                echo "Username: ${gitkey_USR}"
                echo "Password: ${gitkey_PSW}"
            }
        }
    }
}
```
### 4. when
```bash
pipeline{
    agent any
    environment{
        DEPLOY_TO = 'production'
    }
    stages{
        stage('Deploy'){
            when{
                environment name: 'DEPLOY_TO', value: 'production'
            }
            steps{
                echo "deployment done"
            }
        }
    }
}
```
### 5. when not equals
```bash
pipeline{
    agent any
    environment{
        DEPLOY_TO = 'development'
    }
    stages{
        stage('Deploy'){
            when{
                not{
                   equals expected: 'production', actual: "${DEPLOY_TO}"
                }
            }
            steps{
                echo "deployment done"
            }
        }
    }
}
```
### 6. when expression branch - can be used only in MBP
```bash
pipeline{
    agent any
    stages{
        stage('Deploy'){
            when{
                expression{
                    BRANCH_NAME ==~ /(production|staging)/
                }
            }
            steps{
                echo "deployment done"
            }
        }
    }
}
```
### 6.1. when branch - can be used only in MBP
```bash
pipeline{
    agent any
    stages{
        stage('Deploy'){
            when{
                branch 'production'
            }
            steps{
                echo "deployment done"
            }
        }
    }
}
```
### 7. allOf - can be used only in MBP
```bash
pipeline{
    agent any
    environment{
        DEPLOY_TO = 'production'
    }
    stages{
        stage('Deploy'){
            when{
                allOf{
                   branch 'production'
                   environment name: 'DEPLOY_TO', value: 'production'
                }
            }
            steps{
                echo "deployment done"
            }
        }
    }
}
```

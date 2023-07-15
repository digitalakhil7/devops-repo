### Script block to write Groovy code, ${ } - Interpolation
#### Linux commands - sh, echo - inbuilt
```bash
pipeline{
    agent any
    stages{
        stage('Script'){
            steps{
                script{
                    def course = "k8s"
                    if(course == "k8s")
                        echo "you chose ${course}"
                    else
                        echo "you chose nothing"
                }
            }
        }
    }
}
```

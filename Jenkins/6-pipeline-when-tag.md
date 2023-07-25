### when { buildingTag() } - when any tag is present
```bash
pipeline{
    agent any
    stages{
        stage('Build'){
            when{
                buildingTag()
            }
            steps{
                echo "tag present"
            }
        }
    }
}
```
### when { tag "v1.2"} - when particular tag is present
```bash
pipeline{
    agent any
    stages{
        stage('Build'){
            when{
                tag "v1.2"
            }
            steps{
                echo "tag present"
            }
        }
    }
}
```
### when { tag pattern: ""} - when tag with pattern is present
```bash
pipeline{
    agent any
    stages{
        stage('Build'){
            when{
                // ex: v1.2.3
                tag pattern: "v\\d{1,2}.\\d{1,2}.\\d{1,2}", comparator: "REGEXP"
            }
            steps{
                echo "tag present"
            }
        }
    }
}
```

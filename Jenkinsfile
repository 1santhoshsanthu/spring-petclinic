pipeline {
    agent {label 'SPC'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage ("git") {
            steps {
                git url: "https://github.com/1santhoshsanthu/spring-petclinic.git",
                    branch: "main"
            }
        }
    }
}
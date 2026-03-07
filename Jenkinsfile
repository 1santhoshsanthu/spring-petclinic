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
        stage ("scan - sonar") {
            steps {
                withCredentails([string(credentailsId:'SONAR_ID', variable:'SONAR_TOKEN')]){
                withSonarQubeEnv('SONAR'){
                    sh """mvn packege sonar:sonar \
                        -Dsonar.projectKey=1santhoshsanthu_spring-petclinic \
                        -Dsonar.organization=1santhoshsanthu \
                        -Dsonar.host.url=https://sonarcloud.io/ \
                        -Dsonar.login=$SONAR_TOKEN"""
                }
                }
            }
        }
    }
}
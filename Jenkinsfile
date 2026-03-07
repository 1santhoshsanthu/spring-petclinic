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
                withCredentials([string(credentialsId:'SONAR_ID', variable:'SONAR_TOKEN')]){
                withSonarQubeEnv('SONAR'){
                    sh """mvn package sonar:sonar \
                        -Dsonar.projectKey=1santhoshsanthu_spring-petclinic \
                        -Dsonar.organization=1santhoshsanthu \
                        -Dsonar.host.url=https://sonarcloud.io/ \
                        -Dsonar.login=$SONAR_TOKEN"""
                }
                }
            }
        }
    }
    post{
         always{
            archiveArtifacts artifacts: "**/target/*.jar",fingerprint:true
            archiveArtifacts artifacts: "target/surefire-reports/*.xml"
            junit "**/target/surefire-reports/*.xml"
        }
        success {
            echo "pipeline executed"
        }
        failure {
            echo "pipeline failed"
        }
    }
}
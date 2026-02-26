pipeline {
    agent { label 'SPC' }

    triggers {
        pollSCM('H/5 * * * *')   // better than every minute
    }

    tools {
        maven 'Maven-3'   // configure in Global Tool Configuration
    }

    stages {

        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git',
                    branch: 'main'
            }
        }

        stage('Build & Sonar Scan') {
            steps {
                withCredentials([string(credentialsId: 'SONAR', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SONAR') {
                        sh """
                        mvn clean package sonar:sonar \
                        -Dsonar.projectKey=1santhoshsanthu \
                        -Dsonar.organization=1santhoshsanthu \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            junit '**/target/surefire-reports/*.xml'
        }
    }
}

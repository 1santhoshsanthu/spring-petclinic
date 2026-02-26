pipeline{
    agent {label 'SPC'}
    triggers {
        pollSCM('* * * * *')
    }
    stages{
        stage('git checkout'){
            steps{
                git url: 'https://github.com/spring-projects/spring-petclinic.git',
                  branch: 'main'
            }
        }
        stage('build and scan'){
            steps{
                withCredentials([string(credentialsId: 'sonar_id', variable: 'SONAR_TOKEN')]) {
                withSonarQubeEnv('SONAR') {
                sh """mvn package sonar:sonar \
                -Dsonar.projectKey=1santhoshsanthu_spring-petclinic \
                -Dsonar.organisation=1santhoshsanthu \
                -Dsonar.host.url=https://sonarcloud.io/ \
                -Dsonar.login=$SONAR_TOKEN"""
            }
        }
    }
 }
         post {
           always {
            archiveArtifacts artifacts: '**/*.jar'
            junit '**/surefire-reports/*.xml'
        }
    }
}
}

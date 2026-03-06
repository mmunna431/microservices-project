pipeline {
    agent any
    environment{
        scannerHome = tool 'mysonar';
    }
    stages {
        stage('CQA'){
            steps{
                withSonarQubeEnv('mysonar') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=project1"
                    }
                }
            }
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t mmunna431/currencyservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push mmunna431/currencyservice:latest "
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
    environment{
        scannerHome = tool 'mysonar';
    }
    stages {
        stage{
            steps{
                withSonarQubeEnv('mysonar') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=project1"
                    }
                }
            }
        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-2', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://E70948F7AE99A8D8CBBE1479E049FEBA.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl apply -f deployment-service.yml"
                    
                }
            }
        }
        
        stage('verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS-2', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://E70948F7AE99A8D8CBBE1479E049FEBA.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}

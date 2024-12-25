pipeline {
    agent any

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'ngdc-k8s-cluster', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://ngdc-k8s-cluster-a1fc0dced4d0fbdf053d99dd0d50e51d-0000.us-south.stg.containers.appdomain.cloud']]) {
                    sh "kubectl apply -f deployment-service.yml"
                    
                }
            }
        }
        
        stage('verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'ngdc-k8s-cluster', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://ngdc-k8s-cluster-a1fc0dced4d0fbdf053d99dd0d50e51d-0000.us-south.stg.containers.appdomain.cloud']]) {
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        // Point to Minikube's Docker daemon
        DOCKER_HOST = "tcp://$(minikube ip):2376"
        DOCKER_TLS_VERIFY = "1"
        DOCKER_CERT_PATH = "$HOME/.minikube/certs"
        KUBECONFIG = "/var/jenkins_home/.kube/config"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Bhavani2909/Project1.git', credentialsId: 'GitHubPATT'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t demo-app:1.0 .'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                        kubectl apply -f k8s-deployment.yaml
                        kubectl rollout status deployment/demo-app
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed! Check the logs.'
        }
    }
}


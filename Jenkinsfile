

pipeline {
    agent any

    environment {
        // Get Minikube IP and set DOCKER_HOST dynamically
        MINIKUBE_IP = sh(script: "minikube ip", returnStdout: true).trim()
        DOCKER_HOST = "tcp://${MINIKUBE_IP}:2376"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred') // your DockerHub credentials ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'GitHubPATT', url: 'https://github.com/Bhavani2909/Project1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t bhavani2909/demo-app:1.0 ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push bhavani2909/demo-app:1.0"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f k8s-deployment.yaml"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}

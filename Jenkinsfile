pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred') // Jenkins DockerHub credential ID
        IMAGE_NAME = 'bhavani2909/demo-app'
        IMAGE_TAG = '1.0'
        KUBE_CONFIG = credentials('kubeconfig') // Jenkins secret for kubeconfig if needed
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Bhavani2909/Project1.git',
                    credentialsId: 'GitHubPATT'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                    kubectl apply -f k8s-deployment.yaml
                    kubectl rollout status deployment/demo-app
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs.'
        }
    }
}


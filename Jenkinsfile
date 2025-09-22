pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred') // DockerHub credentials in Jenkins
        IMAGE_NAME = 'bhavani2909/demo-app'                 // DockerHub repo name
        IMAGE_TAG = '1.0'                                  // Docker image tag
    }

    stages {

        stage('Checkout') {
            steps {
                // Clone your GitHub repo
                git url: 'https://github.com/Bhavani2909/Project1.git',
                    branch: 'main',
                    credentialsId: 'GitHubPATT'  // GitHub personal access token stored in Jenkins
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image from Dockerfile
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    // Login using Jenkins credentials
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the image to DockerHub
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Create or update the deployment in Minikube
                    sh """
                    kubectl apply -f k8s/deployment.yaml
                    kubectl apply -f k8s/service.yaml
                    """
                }
            }
        }

    } // stages

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}


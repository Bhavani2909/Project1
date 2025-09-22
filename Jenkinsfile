pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred')
        IMAGE_NAME = 'bhavani2909/demo-app'
        IMAGE_TAG = '1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<your-github-repo>/devops-demo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "docker push $IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl set image deployment/demo-app demo-app=$IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }
}


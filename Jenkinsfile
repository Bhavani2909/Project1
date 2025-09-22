pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred') // Jenkins credentials ID
        DOCKER_IMAGE = "bhavani2909/demo-app:1.0"
        GITHUB_REPO = "https://github.com/Bhavani2909/Project1.git"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: "${GITHUB_REPO}",
                    credentialsId: 'GitHubPATT' // your GitHub PAT credential ID
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        kubectl set image deployment/demo-app demo-app=${DOCKER_IMAGE} --record || \
                        kubectl create deployment demo-app --image=${DOCKER_IMAGE}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs."
        }
    }
}



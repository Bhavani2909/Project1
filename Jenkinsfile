pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred')  // Your DockerHub credentials in Jenkins
        KUBECONFIG = "/var/jenkins_home/.kube/config"           // Mounted kubeconfig path
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
                script {
                    sh 'docker build -t bhavani2909/demo-app:1.0 .'
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push bhavani2909/demo-app:1.0'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                        kubectl apply -f k8s-deployment.yaml
                        kubectl apply -f k8s-service.yaml
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed! Check logs.'
        }
    }
}


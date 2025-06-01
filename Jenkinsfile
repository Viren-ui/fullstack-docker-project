peline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub') // Replace 'dockerhub' with your Jenkins Docker Hub credentials ID
        FRONTEND_IMAGE = 'faysalmehedi/frontend:latest'
        BACKEND_IMAGE = 'faysalmehedi/backend:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    docker.build("frontend", "./frontend")
                    docker.build("backend", "./backend")
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        docker.image("frontend").push("latest")
                        docker.image("backend").push("latest")
                    }
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline Complete!'
        }
    }
}
m

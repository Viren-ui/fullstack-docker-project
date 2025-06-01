pipeline {
    agent any
    environment {
        IMAGE_NAME_FRONTEND = 'vire26/frontend-app'
        IMAGE_NAME_BACKEND = 'vire26/backend-app'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Clone the repository
                    git branch: 'main', url: 'https://github.com/Viren-ui/fullstack-docker-project.git'

                }
            }
        }
        stage('Build Frontend Docker Image') {
            steps {
                script {
                    // Navigate to frontend directory and build Docker image
                    dir('frontend') {
                        sh 'docker build -t ${IMAGE_NAME_FRONTEND} .'
                    }
                }
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                script {
                    // Navigate to backend directory and build Docker image
                    dir('backend') {
                        sh 'docker build -t ${IMAGE_NAME_BACKEND} .'
                    }
                }
            }
        }
        stage('Push Docker Images') {
            steps {
                script {
                    // Log in to Docker Hub and push images
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh '''
                            echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                            docker push vire26/frontend-app
                            docker push vire26/backend-app}
                        '''
                    }
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Deploy using docker-compose
                    sh '''
                        docker-compose -f ${DOCKER_COMPOSE_FILE} down
                        docker-compose -f ${DOCKER_COMPOSE_FILE} up -d
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Deployment successful.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}


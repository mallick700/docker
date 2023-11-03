pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker_cred')
    
    }

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker_cred')
        DOCKER_COMPOSE_FILE = 'your-docker-compose.yml'
    }

    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                    // Build Docker images using Docker Compose
                    sh "docker-compose -f ${DOCKER_COMPOSE_FILE} build"

                    // Authenticate with Docker Hub
                    withDockerServer([credentialsId: DOCKER_HUB_CREDENTIALS]) {
                        withDockerRegistry([url: 'https://index.docker.io/v1/']) {
                            // Push all images defined in the Compose file
                            sh "docker-compose -f ${DOCKER_COMPOSE_FILE} push"
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up
            sh 'docker logout'
        }
    }
}

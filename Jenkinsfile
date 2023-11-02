pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker_cred')
    }

    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                    sh 'docker-compose build '
                    def images = [
                        'image1:tag1',
                        'image2:tag2',
                        'image3:tag3',
                        // Add more images as needed
                    ]

                    try {
                        // Authenticate with Docker Hub
                        withDockerServer([credentialsId: DOCKER_HUB_CREDENTIALS]) {
                            withDockerRegistry([url: 'https://index.docker.io/v1/']) {
                                for (def image in images) {
                                    sh "docker pull ${image}" // Pull to ensure layers are available
                                    sh "docker push ${image}"
                                }
                            }
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Failed to build and push Docker images: ${e.message}")
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

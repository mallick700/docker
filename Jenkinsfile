pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker_cred')
    }

    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                    echo 'Starting Docker build'
                    sh 'docker-compose build'
                    echo 'Docker build completed'

                    withDockerRegistry(credentialsId: 'docker_cred', url: 'https://index.docker.io/v1') {
                        echo 'Starting Docker push'
                        sh 'docker-compose push'
                        echo 'Docker push completed'
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}

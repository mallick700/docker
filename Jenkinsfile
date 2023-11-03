pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker_cred')
    }

    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                     withDockerRegistry(credentialsId: 'docker_cred', [url: 'https://index.docker.io/v1/mallick700/dockercompose])' {
                    sh 'docker-compose build '
                    sh 'docker-compose push'
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

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "traefik/whoami"
        CONTAINER_NAME = "whoami-container"
        TRAEFIK_NETWORK = "traefik_network"
    }

    stages {
        stage('Pull Docker Image') {
            steps {
                echo 'Pulling the latest traefik/whoami image...'
                sh "docker pull ${DOCKER_IMAGE}"
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    sh """
                        docker stop ${CONTAINER_NAME} || true
                        docker rm ${CONTAINER_NAME} || true
                    """
                }
            }
        }

        stage('Deploy whoami Container') {
            steps {
                echo 'Deploying whoami container...'
                sh """
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        ${DOCKER_IMAGE}
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Verifying deployment...'
                sh "docker ps -f name=${CONTAINER_NAME}"
            }
        }
    }

    post {
        success {
            echo 'whoami container deployed successfully!'
        }
        failure {
            echo 'Failed to deploy whoami container.'
        }
    }
}
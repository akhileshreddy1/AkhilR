pipeline {
    agent any

    environment {
        // Docker image name
        IMAGE_NAME = 'my-app'
        // Docker Hub credentials ID
        DOCKER_CREDENTIALS = 'docker'
        // Docker Hub repository (adjust to your Docker Hub username)
        DOCKER_REPO = 'akhileshreddy1/my-app'
        // Node.js version
        NODE_VERSION = '16'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository code
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with the tag: repository/image:build_id
                    docker.build("${DOCKER_REPO}/${IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub using provided credentials
                    docker.withRegistry('https://hub.docker.com/r/akhileshreddy1/my-app', DOCKER_CREDENTIALS) {
                        // Push the built image to Docker Hub with the generated tag
                        docker.image("${DOCKER_REPO}/${IMAGE_NAME}:${env.BUILD_ID}").push()
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    // Pull the Docker image from Docker Hub and run it as a container
                    sh """
                    docker pull ${DOCKER_REPO}/${IMAGE_NAME}:${env.BUILD_ID}
                    docker run -d -p 3000:3000 --name nodejs-app ${DOCKER_REPO}/${IMAGE_NAME}:${env.BUILD_ID}
                    """
                }
            }
        }
    }

    post {
        always {
            // Cleanup docker containers and images after the pipeline run
            sh 'docker system prune -f'
        }

        success {
            echo "Deployment completed successfully!"
        }

        failure {
            echo "Deployment failed."
        }
    }
}

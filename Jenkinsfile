pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-app"
        DOCKER_HUB_REPO = "rajitha390/my-app"
        DOCKER_REGISTRY_CREDENTIALS = "dockerhubcredentials"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://https://github.com/rajitha690/my-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_HUB_REPO}:latest ."
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: DOCKER_REGISTRY_CREDENTIALS, url: '') {
                        sh "docker push ${DOCKER_HUB_REPO}:latest"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop ${IMAGE_NAME} || true && docker rm ${IMAGE_NAME} || true"
                    sh "docker run -d -p 3000:3000 --name ${IMAGE_NAME} ${DOCKER_HUB_REPO}:latest"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! 🎉"
        }
        failure {
            echo "Deployment failed! ❌"
        }
    }
}

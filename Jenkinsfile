pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')  // Jenkins credentials ID for Docker Hub
        IMAGE_NAME = "rashmitha1812/react-app:latest"
        CONTAINER_NAME = "react-app"
    }

    stages {
        stage('Pull Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    // Login to Docker Hub non-interactively
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    // Pull the latest image from Docker Hub
                    sh "docker pull ${IMAGE_NAME}"
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                sh """
                # Stop existing container if it exists
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                
                # Run new container on port 80
                docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}
                """
            }
        }
    }
}

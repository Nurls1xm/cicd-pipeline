pipeline {
    agent any

    tools {
        nodejs 'Node 7.8.0'
    }

    environment {
        PORT = "3000"
        IMAGE_NAME = "nodemain:v1.0"
        CONTAINER_NAME = "nodemain-container"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Branch: ${env.BRANCH_NAME}"
            }
        }
        stage('Build') {
            steps {
                sh 'chmod +x scripts/build.sh && ./scripts/build.sh'
            }
        }
        stage('Test') {
            steps {
                sh 'chmod +x scripts/test.sh && ./scripts/test.sh'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${IMAGE_NAME} .
                    docker images | grep nodemain
                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p ${PORT}:3000 ${IMAGE_NAME}
                    echo "Deployed at http://localhost:${PORT}"
                """
            }
        }
    }
}

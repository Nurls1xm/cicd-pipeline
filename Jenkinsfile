pipeline {
    agent any
<<<<<<< HEAD

    tools {
        nodejs 'Node 7.8.0'
    }

    environment {
        PORT = "3001"
        IMAGE_NAME = "nodedev:v1.0"
        CONTAINER_NAME = "nodedev-container"
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
                    docker images | grep nodedev
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
=======
    
    environment {
        DOCKER_REGISTRY = "localhost:5000"
        IMAGE_NAME = "nodeapp"
        IMAGE_TAG = "v1.0"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo "========== Checkout Stage =========="
                checkout scm
                sh 'git branch -a'
            }
        }
        
        stage('Build') {
            steps {
                echo "========== Build Stage =========="
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                echo "========== Test Stage =========="
                sh 'npm test || true'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo "========== Build Docker Image =========="
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker build -t nodemain:v1.0 .'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker build -t nodedev:v1.0 .'
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo "========== Deploy Stage =========="
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh '''
                            docker stop nodeapp-main || true
                            docker rm nodeapp-main || true
                            docker run -d -p 3000:3000 --name nodeapp-main nodemain:v1.0
                        '''
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh '''
                            docker stop nodeapp-dev || true
                            docker rm nodeapp-dev || true
                            docker run -d -p 3001:3000 --name nodeapp-dev nodedev:v1.0
                        '''
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo "Pipeline finished"
        }
    }
>>>>>>> 179c4571c017d25992cbded908f3f2b733c7bafd
}

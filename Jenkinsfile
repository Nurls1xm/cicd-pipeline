pipeline {
    agent any
    
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
}

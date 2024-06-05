pipeline {
    agent any

    environment {
        // Define Docker Hub credentials and repository details
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials'
        DOCKER_HUB_REPO = 'fatimaqurban'
        // Define project directories
        BACKEND_SERVICE1_DIR = 'i211195-backend-Auth'
        BACKEND_SERVICE2_DIR = 'i211195-backend-Classrooms'
        BACKEND_SERVICE3_DIR = 'i211195-backend-event-bus'
        FRONTEND_SERVICE_DIR = 'i211195-frontend-client'
    }

    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com/NUCESFAST/scd-final-lab-exam-Fatimaqurban', branch: 'master'
            }
        }
        
        stage('Build Docker images') {
            parallel {
                stage('Build Backend Service 1') {
                    steps {
                        script {
                            dir(BACKEND_SERVICE1_DIR) {
                                sh 'docker build -t fatimaqurban/auth-deployment .'
                            }
                        }
                    }
                }
                stage('Build Backend Service 2') {
                    steps {
                        script {
                            dir(BACKEND_SERVICE2_DIR) {
                                sh 'docker build -t fatimaqurban/classroom-deployment .'
                            }
                        }
                    }
                }
                stage('Build Backend Service 3') {
                    steps {
                        script {
                            dir(BACKEND_SERVICE3_DIR) {
                                sh 'docker build -t fatimaqurban/eventbus-deployment .'
                            }
                        }
                    }
                }
                stage('Build Frontend Service') {
                    steps {
                        script {
                            dir(FRONTEND_SERVICE_DIR) {
                                sh 'docker build -t fatimaqurban/client-deployment .'
                            }
                        }
                    }
                }
            }
        }
        
        stage('Push Docker images to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER_HUB_CREDENTIALS') {
                        sh 'docker push fatimaqurban/auth-deployment'
                        sh 'docker push fatimaqurban/classroom-deployment'
                        sh 'docker push fatimaqurban/eventbus-deployment'
                        sh 'docker push fatimaqurban/client-deployment'
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

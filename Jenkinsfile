pipeline {
    environment {
        registryCredential = 'docker-hub' // Make sure this is the ID of your Docker Hub credentials in Jenkins
        DOCKER_REGISTRY = 'khadijahmehmood/scd-final-lab-exam'
    }
    agent any
    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'master', url: 'https://github.com/NUCESFAST/scd-final-lab-exam-Khadijahmehmoodd.git'
                }
            }
        }
        stage('Build and Push Docker Images') {
            parallel {
                stage('Auth Service') {
                    stages {
                        stage('Build Auth Service') {
                            steps {
                                script {
                                    buildDockerImage('Auth', 'auth-service', '600970')
                                }
                            }
                        }
                        stage('Push Auth Service') {
                            steps {
                                script {
                                    pushDockerImage('auth-service')
                                }
                            }
                        }
                    }
                }
                stage('Classrooms Service') {
                    stages {
                        stage('Build Classroom Service') {
                            steps {
                                script {
                                    buildDockerImage('Classrooms', 'classrooms-service', '500970')
                                }
                            }
                        }
                        stage('Push Classrooms Service') {
                            steps {
                                script {
                                    pushDockerImage('classrooms-service')
                                }
                            }
                        }
                    }
                }
                stage('Event-Bus Service') {
                    stages {
                        stage('Build Event-Bus Service') {
                            steps {
                                script {
                                    buildDockerImage('event-bus', 'event-bus-service', '400970')
                                }
                            }
                        }
                        stage('Push Event-Bus Service') {
                            steps {
                                script {
                                    pushDockerImage('event-bus-service')
                                }
                            }
                        }
                    }
                }
                stage('Post Service') {
                    stages {
                        stage('Build Post Service') {
                            steps {
                                script {
                                    buildDockerImage('Post', 'post-service', '300970')
                                }
                            }
                        }
                        stage('Push Post Service') {
                            steps {
                                script {
                                    pushDockerImage('post-service')
                                }
                            }
                        }
                    }
                }
                stage('Frontend Service') {
                    stages {
                        stage('Build Frontend Service') {
                            steps {
                                script {
                                    buildDockerImage('client', 'frontend-service', '200970')
                                }
                            }
                        }
                        stage('Push Frontend Service') {
                            steps {
                                script {
                                    pushDockerImage('frontend-service')
                                }
                            }
                        }
                    }
                }
            }
        }
    }
    post {
        success {
            emailext(
                to: 'khadijamehmood77@gmail.com',
                subject: 'Update on Image Build',
                body: 'The Docker images have been successfully built and pushed.'
            )
        }
    }
}

def buildDockerImage(directory, imageName, port) {
    withCredentials([usernamePassword(credentialsId: env.registryCredential, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
        dir(directory) {
            sh "docker build -t ${env.DOCKER_REGISTRY}/${imageName}:${env.BUILD_ID} --build-arg PORT=${port} ."
        }
    }
}

def pushDockerImage(imageName) {
    withCredentials([usernamePassword(credentialsId: env.registryCredential, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
        sh "docker push ${env.DOCKER_REGISTRY}/${imageName}:${env.BUILD_ID}"
    }
}

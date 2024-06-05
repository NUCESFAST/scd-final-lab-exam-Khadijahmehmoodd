pipeline {
    environment {
        registryCredential = 'Docker-hubb' // Jenkins credentials ID for Docker Hub
        DOCKER_REGISTRY = 'khadijahmehmood/scd-final-lab-exam'
        GIT_REPO = 'https://github.com/NUCESFAST/scd-final-lab-exam-Khadijahmehmoodd.git'
    }
    agent any
    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'master', url: "${env.GIT_REPO}"
                }
            }
        }
        stage('Build and Push Docker Images') {
            parallel {
                stage('Auth Service') {
                    stages {
                        stage('Build Auth Service') {
                            steps {
                                dir('Auth') {
                                    script {
                                        buildDockerImage('auth-service')
                                    }
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
                        stage('Build Classrooms Service') {
                            steps {
                                dir('Classrooms') {
                                    script {
                                        buildDockerImage('classrooms-service')
                                    }
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
                                dir('event-bus') {
                                    script {
                                        buildDockerImage('event-bus-service')
                                    }
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
                                dir('Post') {
                                    script {
                                        buildDockerImage('post-service')
                                    }
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
                                dir('client') {
                                    script {
                                        buildDockerImage('frontend-service')
                                    }
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

def buildDockerImage(imageName) {
    docker.build("${DOCKER_REGISTRY}/${imageName}")
}

def pushDockerImage(imageName) {
    docker.withRegistry('', registryCredential) {
        docker.image("${DOCKER_REGISTRY}/${imageName}").push()
    }
}

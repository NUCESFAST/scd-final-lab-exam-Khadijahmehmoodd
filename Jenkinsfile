pipeline {
    environment {
        registryCredential = 'Docker-hubb' // Jenkins credentials ID for Docker Hub
        DOCKER_REGISTRY = 'khadijahmehmood/scd-final-lab-examm'
        TAG = 'LATEST' // Corrected TAG definition
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
                stage('i200970 Auth Service') {
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
                        stage(' 20i-0970 Push Auth Service') {
                            steps {
                                script {
                                    pushDockerImage('auth-service')
                                }
                            }
                        }
                    }
                }
                stage('20i-0970 Classrooms Service') {
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
                        stage('20i-0970 Push Classrooms Service') {
                            steps {
                                script {
                                    pushDockerImage('classrooms-service')
                                }
                            }
                        }
                    }
                }
                stage('20i-0970 Event-Bus Service') {
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
                        stage('20i-0970 Push Event-Bus Service') {
                            steps {
                                script {
                                    pushDockerImage('event-bus-service')
                                }
                            }
                        }
                    }
                }
                stage(' 20i-0970 Post Service') {
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
                        stage('20i-0970 Push Post Service') {
                            steps {
                                script {
                                    pushDockerImage('post-service')
                                }
                            }
                        }
                    }
                }
                stage('20i-0970 Frontend Service') {
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
                        stage('20i-0970 Push Frontend Service') {
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
    def image = docker.build("${env.DOCKER_REGISTRY}/${imageName}:${TAG}") // Added TAG to the image name
    return image
}

def pushDockerImage(imageName) {
    try {
        docker.withRegistry('', registryCredential) {
            def image = docker.image("${env.DOCKER_REGISTRY}/${imageName}:${TAG}") // Added TAG to the image name
            image.push()
        }
    } catch (Exception e) {
        println "Failed to push image: ${e.message}"
    }
}

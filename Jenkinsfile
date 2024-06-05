pipeline {
    environment {
        registryCredential = 'Docker-hubb' // Jenkins credentials ID for Docker Hub
        DOCKER_REGISTRY = 'khadijahmehmood/scd-final-lab-examm'
        TAG = 'latest' // Corrected TAG definition
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
                    steps {
                        script {
                            dir('Auth') {
                                buildDockerImage('auth-service')
                                pushDockerImage('auth-service')
                            }
                        }
                    }
                }
                stage('20i-0970 Classrooms Service') {
                    steps {
                        script {
                            dir('Classrooms') {
                                buildDockerImage('classrooms-service')
                                pushDockerImage('classrooms-service')
                            }
                        }
                    }
                }
                stage('20i-0970 Event-Bus Service') {
                    steps {
                        script {
                            dir('event-bus') {
                                buildDockerImage('event-bus-service')
                                pushDockerImage('event-bus-service')
                            }
                        }
                    }
                }
                stage(' 20i-0970 Post Service') {
                    steps {
                        script {
                            dir('Post') {
                                buildDockerImage('post-service')
                                pushDockerImage('post-service')
                            }
                        }
                    }
                }
                stage('20i-0970 Frontend Service') {
                    steps {
                        script {
                            dir('client') {
                                buildDockerImage('frontend-service')
                                pushDockerImage('frontend-service')
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
    docker.withRegistry('', registryCredential) {
        def image = docker.image("${env.DOCKER_REGISTRY}/${imageName}:${TAG}") // Added TAG to the image name
        image.push()
    }
}

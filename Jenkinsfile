pipeline {
    agent any
    
    environment {
        registryCredential = 'Docker-hubb' // Jenkins credentials ID for Docker Hub
        DOCKER_REGISTRY = 'khadijahmehmood/scd-final-lab-examm' // Docker registry/repository
        TAG = 'latest' // Tag for Docker image
        GIT_REPO = 'https://github.com/NUCESFAST/scd-final-lab-exam-Khadijahmehmoodd.git' // Git repository URL
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'master', url: env.GIT_REPO
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_REGISTRY}:${env.TAG}")
                    docker.withRegistry('', registryCredential) {
                        docker.image("${env.DOCKER_REGISTRY}:${env.TAG}").push()
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
                body: 'The Docker image has been successfully built and pushed.'
            )
        }
    }
}

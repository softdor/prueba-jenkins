pipeline {
    agent any

    environment {
        // Define variables
        REMOTE_HOST = 'jenkins.softdor.com'
        GIT_REPO_URL = 'https://github.com/softdor/prueba-jenkins.git'
        IMAGE_NAME = 'web-simple'
        DOCKER_CREDENTIALS_ID = 'sd-docker-hub-credentials'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code to Jenkins workspace
                git "${GIT_REPO_URL}"
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t softdor/web-simple .'
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def dockerImage = docker.image("${IMAGE_NAME}")
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}

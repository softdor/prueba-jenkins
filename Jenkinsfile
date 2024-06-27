pipeline {
    agent any

    environment {
        // Define variables
        REMOTE_HOST = 'jenkins.softdor.com'
        GIT_REPO_URL = 'https://github.com/softdor/prueba-jenkins.git'
        IMAGE_NAME = 'web-simple'
        CONTAINER_NAME = 'web'
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
                script {
                    app = docker.build('softdor/prueba-jenkins')
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'git') {            
                        app.push("${env.BUILD_NUMBER}")            
                        app.push("latest")
                    }
                }
            }
        }
    }
}

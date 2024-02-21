pipeline {
    agent any

    environment {
        // Define variables
        REMOTE_HOST = '179.24.105.234'
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

        stage('Build and Deploy') {
            steps {
                // Using SSH Agent to connect to the remote server
                sshagent(['your_ssh_credentials_id']) {
                    // Commands to execute on the remote server
                    script {
                        sh """
                        ssh -o StrictHostKeyChecking=no ${REMOTE_HOST} << EOF
                            # Navigate to the project directory or clone the repo
                            if [ -d "project-directory" ]; then
                                cd project-directory && git pull
                            else
                                git clone ${GIT_REPO_URL} project-directory
                                cd project-directory
                            fi

                            # Build Docker image
                            docker build -t ${IMAGE_NAME} .

                            # Stop and remove the previous container
                            docker stop ${CONTAINER_NAME} || true
                            docker rm ${CONTAINER_NAME} || true

                            # Run a new container with the new image
                            docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}
                        EOF
                        """
                    }
                }
            }
        }
    }
}


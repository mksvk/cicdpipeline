pipeline {
    agent kk

    environment {
        DOCKER_REPO = "mksvk/pipeline"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the Git repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image from the Dockerfile in the repository
                script {
                    def dockerImage = docker.build("${DOCKER_REPO}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Push the Docker image to Docker Hub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Running Container') {
          steps {
            script {
              sh "docker run -d -p 3002:80 ${registry}" + ":$BUILD_NUMBER" 
            }
            }
        }
     
        }
}


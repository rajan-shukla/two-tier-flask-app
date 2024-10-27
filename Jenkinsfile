pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'rajan715/todo'
        DOCKER_HUB_CREDENTIALS_ID = 'todo' // Add Docker credentials in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/rajan-shukla/two-tier-flask-app'
            }
        }

        stage('Check Docker Daemon') {
            steps {
                script {
                    // Check if Docker daemon is running
                    def dockerStatus = sh(script: 'docker info > /dev/null 2>&1', returnStatus: true)
                    if (dockerStatus != 0) {
                        error "Docker daemon is not running. Please start Docker and try again."
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build("${DOCKER_HUB_REPO}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in and push image to Docker Hub
                    docker.withRegistry('', DOCKER_HUB_CREDENTIALS_ID) {
                        docker.image("${DOCKER_HUB_REPO}:${env.BUILD_ID}").push()
                    }
                }
            }
        }

        stage('Deploy to Localhost') {
            steps {
                script {
                    // Run the app as a container and expose it on port 8080
                    bat "docker run --rm -d -p 9090:8000 ${DOCKER_HUB_REPO}:${env.BUILD_ID}"
                }
            }
        }
    }
}

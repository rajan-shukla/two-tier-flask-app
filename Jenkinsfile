pipeline {
   agent any
   environment {
       DOCKER_HUB_REPO = 'rajan715/two-flask-app'
       DOCKER_HUB_CREDENTIALS_ID = 'two-flask-app' // Add Docker credentials in Jenkins
   }
   stages {
       stage('Clone Repository') {
           steps {
               // Clone the repository
               git branch: 'master', url: 'https://github.com/rajan-shukla/two-tier-flask-app'
           }
       }
       stage('Build Docker Image') {
           steps {
               // Build Docker image
               script {
                   docker.build("${DOCKER_HUB_REPO}:${env.BUILD_ID}")
               }
           }
       }
       stage('Push Docker Image') {
           steps {
               // Log in and push image to Docker Hub
               script {
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
                   bat "docker run --rm -d -p 9090:3000 ${DOCKER_HUB_REPO}:${env.BUILD_ID}"
               }
           }
       }
   }
}

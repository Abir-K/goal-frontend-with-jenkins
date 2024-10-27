pipeline {
    agent any
    environment {
        // Set Docker registry details here
        DOCKER_REGISTRY = "kaderdevops"
        DOCKER_CREDENTIALS = "dockeraccess"
    }
    stages {
        stage('Build Frontend') {
            steps {
                script {
                    // Build the frontend Docker image
                    sh 'ls'
                    docker.build("${DOCKER_REGISTRY}/goal-front-jenkins:jenkins", ".")
                }
            }
        }
        
        stage('Pushing Frontend Images') {
            steps {
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", "${DOCKER_CREDENTIALS}") {
                        docker.image("${DOCKER_REGISTRY}/goal-front-jenkins:jenkins").push()
                 }
             }
         }
     }
  }
}

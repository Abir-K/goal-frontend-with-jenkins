pipeline {
    agent any
    environment {
        DOCKER_USERNAME = "kaderdevops"  // Docker Hub username
        DOCKER_CREDENTIALS = "docker_jenkins_access" // Jenkins credentials ID
        IMAGE_NAME = "goal-front-jenkins"
        TAG = "deploy"
    }
    stages {
        stage('Build Frontend') {
            steps {
                script {
                    // Build the frontend Docker image
                    docker.build("${DOCKER_USERNAME}/${IMAGE_NAME}:${TAG}", ".")
                }
            }
        }
        
        stage('Push Frontend Image') {
            steps {
                script {
                    // Use the correct Docker Hub registry URL for login
                    docker.withRegistry("https://index.docker.io/v1/", "${DOCKER_CREDENTIALS}") {
                        docker.image("${DOCKER_USERNAME}/${IMAGE_NAME}:${TAG}").push()
                    }
                }
            }
        }
        stage('Update Deployment File') {
        environment {
            GIT_REPO_NAME = "goal-frontend-with-jenkins"
            GIT_USER_NAME = "Abir-K"
        }
        steps {
            withCredentials([usernamePassword(credentialsId: 'hellogithub', passwordVariable: 'pass', usernameVariable: 'uname')]) {
                sh '''
                    git config user.email "abirbeatz@gmail.com"
                    git config user.name "Abir-K"
                    BUILD_NUMBER=${TAG}
                    git checkout main
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" argocd_deployment/deployment.yaml
                    git add argocd_deployment/deployment.yaml
                    git commit -m "New Tag_${BUILD_NUMBER}"
                    git push https://$uname:$pass@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} main
                '''
            }
        }
    }
    }
}

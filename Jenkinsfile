pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "neongutsyyy/minecraft-k8s:${BUILD_NUMBER}"
        DOCKER_HUB_CREDENTIALS = 'dockerhub-credentials'
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE, "-f docker/Dockerfile .")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Update Kubernetes deployment with the new image
                    sh "kubectl set image deployment/minecraft minecraft=${DOCKER_IMAGE}"
                }
            }
        }
    }
    
    post {
        always {
            // Clean up the local Docker image
            sh "docker rmi ${DOCKER_IMAGE}"
        }
    }
}

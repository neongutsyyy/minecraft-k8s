pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "neongutsyyy/minecraft-k8s:${BUILD_NUMBER}"
        DOCKER_HUB_CREDENTIALS = 'dockerhub-credentials'
    }
    
    stages {
        // stage('Build Docker Image') {
        //     steps {
        //         script {
        //             docker.build(DOCKER_IMAGE, "-f docker/Dockerfile .")
        //         }
        //     }
        // }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Update Kubernetes deployment with the new image
                    sh "cat $KUBECONFIG"
                    sh "kubectl set image deployment/minecraft minecraft=${DOCKER_IMAGE} --v=6"
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

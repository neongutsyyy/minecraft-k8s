pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Apply Kubernetes Manifests') {
            steps {
                script {
                    sh 'kubectl apply -f kubernetes/'
                }
            }
        }
        
        stage('Trigger ArgoCD Sync') {
            steps {
                script {
                    sh 'argocd app sync minecraft'
                }
            }
        }
    }
}
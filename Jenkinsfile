pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:$PATH" // Ensure kubectl path is included in PATH
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                // Use Ansible plugin to run playbook
                ansiblePlaybook(
                    playbook: 'ansible/minecraft-playbook.yaml',
                    inventory: 'localhost,'
                )
            }
        }

        stage('Apply Kubernetes Manifests') {
            steps {
                script {
                    // Apply Kubernetes manifests using kubectl
                    sh 'kubectl apply -f kubernetes/'
                }
            }
        }

        stage('Trigger ArgoCD Sync') {
            steps {
                script {
                    // Trigger ArgoCD sync
                    sh 'argocd app sync minecraft'
                }
            }
        }
    }
}

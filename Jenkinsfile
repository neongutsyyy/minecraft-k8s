pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:$PATH"
        KUBECONFIG = credentials('kubeconfig-credential-id') // Add this line
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                ansible-galaxy collection install community.kubernetes
                ansible-galaxy collection install community.general
                '''
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook(
                    playbook: 'ansible/minecraft-playbook.yaml',
                    inventory: 'localhost,',
                    extras: '-e "kubernetes_enabled=true"',
                    colorized: true
                )
            }
        }

        stage('Apply Kubernetes Manifests') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig-credential-id']) {
                    sh 'kubectl apply -f kubernetes/'
                }
            }
        }

        stage('Trigger ArgoCD Sync') {
            steps {
                withCredentials([string(credentialsId: 'argocd-auth-token', variable: 'ARGOCD_AUTH_TOKEN')]) {
                    sh 'argocd app sync minecraft --auth-token $ARGOCD_AUTH_TOKEN'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
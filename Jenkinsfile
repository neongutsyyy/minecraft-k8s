pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install kubectl') {
            steps {
                script {
                    sh '''
                        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                        chmod +x kubectl
                        sudo mv kubectl /usr/local/bin/
                        kubectl version --client
                    '''
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook(
                    playbook: 'ansible/minecraft-playbook.yml',
                    inventory: 'localhost,'
                )
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
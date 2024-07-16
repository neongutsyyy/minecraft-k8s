pipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your/minecraft-server-repo.git'
            }
        }
        
        stage('Deploy with Ansible') {
            steps {
                ansiblePlaybook(
                    playbook: 'ansible/minecraft.yml',
                    inventory: 'ansible/inventory'
                )
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                kubernetesDeploy(
                    configs: 'kubernetes/*.yaml',
                    kubeconfigId: 'kubeconfig'
                )
            }
        }
    }
}
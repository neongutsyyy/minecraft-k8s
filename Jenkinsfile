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
                        # Download kubectl
                        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                        
                        # Make kubectl executable
                        chmod +x kubectl
                        
                        # Move kubectl to a directory in PATH
                        mkdir -p $HOME/bin
                        mv kubectl $HOME/bin/
                        
                        # Add the bin directory to PATH
                        export PATH=$PATH:$HOME/bin
                        
                        # Verify kubectl installation
                        $HOME/bin/kubectl version --client
                    '''
                }
            }
        }

        stage('Install Ansible') {
            steps {
                script {
                    // Check if Ansible is installed
                    def ansibleInstalled = sh(script: 'ansible --version', returnStatus: true)
                    
                    if (ansibleInstalled != 0) {
                        // Install Ansible using pip
                        sh '''
                            pip install --user ansible
                            export PATH="$HOME/.local/bin:$PATH"
                        '''
                    } else {
                        echo 'Ansible is already installed.'
                    }
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Execute your Ansible playbook
                    sh 'ansible-playbook ansible/minecraft-playbook.yaml -i localhost,'
                }
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
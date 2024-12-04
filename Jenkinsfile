pipeline {
    agent any

    stages {
        stage('terraform plan') {
            steps {
                echo 'terraform plan'
                script {
                    dir('infrastructucture/terraform') {
                        sh "terraform --version"
                        sh "terraform init"
                        sh "terraform plan"
                    }
                    input(message: "Are you sure you want to proceed with applying the infrastructure changes?", ok: "Proceed")
                }
            }
        }
        stage('terraform apply') {
            steps {
                echo 'terraform apply'
                script {
                    dir('infrastructucture/terraform') {
                        def action = input message: 'Select Action', parameters: [
                            choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Choose whether to apply or destroy the infrastructure')
                        ]
                        if (action == 'apply') {
                            sh 'terraform apply --auto-approve'
                        } else if (action == 'destroy') {
                            sh 'terraform destroy --auto-approve'
                        }
                    }
                }
            }
        }
        stage('Setup Kubernetes Config') {
            steps {
                echo 'Setup Kubernetes Config'
                script {
                    sh 'aws eks update-kubeconfig --name myapp-eks-cluster'
                    sh 'kubectl get ns'
                }
            }
        }
        stage('deploy tools') {
            steps {
                echo 'deploy tools'
                script {
                    dir('infrastructucture/tools') {
                        sh 'kubectl create namespace argocd'
                        sh 'kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml'
                        sh 'kubectl create namespace argo-rollouts'
                        sh 'kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml'
                        sh 'kubectl apply -f .'
                    }
                }
            }
        }
        stage('xxx') {
            steps {
                echo 'xxx'
                script {}
            }
        }
        stage('xxx') {
            steps {
                echo 'xxx'
                script {}
            }
        }
        stage('xxx') {
            steps {
                echo 'xxx'
                script {}
            }
        }
        stage('xxx') {
            steps {
                echo 'xxx'
                script {}
            }
        }
        
    }
}

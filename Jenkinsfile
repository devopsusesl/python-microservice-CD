pipeline {
    agent any

    stages {
        stage('Terraform Plan') {
            steps {
                echo 'Terraform Plan'
                script {
                    dir('infrastructure/terraform') {
                        sh "terraform --version"
                        sh "terraform init"
                        sh "terraform plan"
                    }
                    input(message: "Are you sure you want to proceed with applying the infrastructure changes?", ok: "Proceed")
                }
            }
        }
        stage('Terraform Apply') {
            steps {
                echo 'Terraform Apply'
                script {
                    dir('infrastructure/terraform') {
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
                echo 'Setting up Kubernetes Config'
                script {
                    sh 'aws eks update-kubeconfig --name myapp-eks-cluster'
                    sh 'kubectl get ns'
                }
            }
        }
        stage('Deploy Tools') {
            steps {
                echo 'Deploy Tools'
                script {
                    dir('infrastructure/cluster-tools') {
                        sh 'kubectl create namespace argocd'
                        sh 'kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml'
                        sh 'kubectl create namespace argo-rollouts'
                        sh 'kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml'
                        sh 'kubectl apply -f .'
                    }
                }
            }
        }
        stage('Deploy Application') {  // Replace xxx with an appropriate stage name
            steps {
                echo 'Deploying Application'
                // Add deployment steps
            }
        }
        stage('Test Application') {  // Replace xxx with another appropriate stage name
            steps {
                echo 'Testing Application'
                // Add testing steps
            }
        }
        stage('Monitor Application') {  // Another meaningful name
            steps {
                echo 'Monitoring Application'
                // Add monitoring steps
            }
        }
    }
}

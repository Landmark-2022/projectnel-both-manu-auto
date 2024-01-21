pipeline {
    agent any

    parameters {
        string(name: 'AWS_REGION', defaultValue: 'us-east-2', description: 'AWS Region for deployment')
        booleanParam(name: 'APPROVE', defaultValue: false, description: 'Approve Terraform changes')
    }

    stages {
        stage('Terraform Apply') {
            steps {
                script {
                    // Change to your Terraform directory
                    dir('1-pet-infra/terraform') {
                        sh 'terraform init'
                        
                        if (params.APPROVE) {
                            input "Do you want to apply Terraform changes? (Requires approval)"
                            sh "terraform apply -auto-approve -var 'region=${params.AWS_REGION}'"
                        } else {
                            echo "Terraform apply skipped. Set 'APPROVE' parameter to true to apply changes."
                        }
                    }
                }
            }
        }

        stage('Ansible Configuration') {
            steps {
                script {
                    // Change to your Ansible directory
                    dir('1-pet-infra/ansible') {
                        sh 'ansible-playbook --vault-id .password site.yml'
                    }
                }
            }
        }
    }

post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
pipeline {
    agent any

    environment {
        TERRAFORM_VERSION = '1.4.0'  // Specify the Terraform version you need
        ARM_SUBSCRIPTION_ID = "${env.AZURE_SUBSCRIPTION_ID}"
        ARM_CLIENT_ID = "${env.AZURE_CLIENT_ID}"
        ARM_CLIENT_SECRET = "${env.AZURE_CLIENT_SECRET}"
        ARM_TENANT_ID = "${env.AZURE_TENANT_ID}"
        
        
          // Replace with your AWS credentials
        TF_VAR_region = 'Central US'  // AWS region (if you're using AWS)
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the Git repository 
                checkout scm
            }
        }

        stage('Install Terraform') {
            steps {
                script {
                    // Install Terraform if it's not installed (can be skipped if pre-installed)
                        sh 'rm -f /usr/local/bin/terraform'
                        sh 'curl -LO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip'
                        sh 'unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip'
                        sh 'mv terraform /usr/local/bin/'
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    // Initialize Terraform (downloads required providers, etc.)
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Run terraform plan and show the changes it will make
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Apply the Terraform plan to provision the infrastructure
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }

        // stage('Terraform Destroy (Optional)') {
        //     when {
        //         branch 'main'  // Destroy only on main branch
        //     }
        //     steps {
        //         script {
        //             // Optionally, add a stage to destroy infrastructure after applying
        //             // Uncomment the next line if you want to destroy infrastructure after the apply
        //             // sh 'terraform destroy -auto-approve'
        //         }
        //     }
        // }
    }

    post {
        success {
            echo 'Terraform pipeline executed successfully!'
        }
        failure {
            echo 'Terraform pipeline failed. Please check the logs.'
        }
    }
}

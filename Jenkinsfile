pipeline {
    agent any
    environment {
        PATH = "/var/jenkins_home/bin:$PATH"
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    stages {
        stage('Build Dockerfile') {
            steps {
                script {
                    // Build Dockerfile
                    sh 'docker build -t myapp .'
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // Use AWS CLI to get ECR login password
                    def awsPassword = sh(script: 'aws ecr get-login-password --region us-east-1', returnStdout: true).trim()

                    // Perform Docker login non-interactively
                    sh "echo '${awsPassword}' | docker login --username AWS --password-stdin 949609334280.dkr.ecr.us-east-1.amazonaws.com"

                    // Tag the image
                    sh 'docker tag myapp:latest 949609334280.dkr.ecr.us-east-1.amazonaws.com/my-ecr-repository:latest'  // Corrected tag

                    // Push the image to ECR
                    sh 'docker push 949609334280.dkr.ecr.us-east-1.amazonaws.com/my-ecr-repository:latest'  // Corrected tag
                }
            }
        }
    } //stages end

    post {
        success {
            // Send success notifications (e.g., Slack, email) if the pipeline succeeds
            echo 'Pipeline succeeded!'
        }
        failure {
            // Send failure notifications (e.g., Slack, email) if the pipeline fails
            echo 'Pipeline failed!'
        }
    }
} //pipeline end

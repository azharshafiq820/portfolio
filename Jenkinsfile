pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'  // Set AWS region
        S3_BUCKET = 'jenkins7865' // Your S3 bucket
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Validate Static Website') {
            steps {
                echo "No build required for HTML/CSS/JS."
            }
        }

        stage('Deploy to AWS S3') {
            steps {
                withAWS(credentials: 'aws-credentials-id', region: "${AWS_REGION}") {
                    sh """
                    if ! command -v aws &> /dev/null; then
                        echo "Installing AWS CLI..."
                        sudo apt-get update && sudo apt-get install awscli -y
                    else
                        echo "AWS CLI is already installed."
                    fi

                    aws s3 sync . s3://${S3_BUCKET} --delete
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}

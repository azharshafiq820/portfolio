pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        S3_BUCKET = 'jenkins7865'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to AWS S3') {
            steps {
                withAWS(region: "${AWS_REGION}", credentials: 'aws-credentials-id') {
                    sh '''
                    if ! command -v aws &> /dev/null; then
                        echo "AWS CLI not found. Installing..."
                        if ! command -v unzip &> /dev/null; then
                            echo "Installing unzip..."
                            sudo apt-get update && sudo apt-get install unzip -y
                        fi
                        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                        unzip awscliv2.zip
                        ./aws/install
                    else
                        echo "AWS CLI is already installed."
                    fi

                    aws s3 sync . s3://${S3_BUCKET} --delete
                    '''
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

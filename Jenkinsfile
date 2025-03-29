pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY = credentials('AWS_ACCESS_KEY')
        AWS_SECRET_KEY = credentials('AWS_SECRET_KEY')
        AWS_REGION = 'us-east-1' // Change to your AWS region
        S3_BUCKET = 'jenkins7658'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build (Validate HTML/CSS/JS)') {
            steps {
                echo "No build required for static websites."
            }
        }

        stage('Deploy to AWS S3') {
            steps {
                withEnv([
                  "AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY}",
                  "AWS_SECRET_ACCESS_KEY=${AWS_SECRET_KEY}",
                  "AWS_DEFAULT_REGION=${AWS_REGION}"
                ]) {
                    sh """
                    sudo apt-get install awscli -y
                    aws s3 sync . s3://$S3_BUCKET --delete
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment Successful!"
        }
        failure {
            echo "Deployment Failed!"
        }
    }
}

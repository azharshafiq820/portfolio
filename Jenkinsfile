pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key')    // AWS Access Key (from Jenkins credentials)
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key') // AWS Secret Key (from Jenkins credentials)
        AWS_DEFAULT_REGION = 'us-east-1'                     // Change to your AWS region
        S3_BUCKET_NAME = 'jenkins7658'               // Replace with your S3 bucket name
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Fetching the code from repository...'
                checkout scm // Pulls the code from your repository
            }
        }

        stage('Validate HTML & CSS') {
            steps {
                echo 'Validating HTML and CSS...'
                sh '''
                # Install HTML and CSS linters (optional)
                sudo apt-get update && sudo apt-get install -y tidy
                tidy -errors index.html || true
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                echo 'Deploying website to AWS S3...'
                sh '''
                aws s3 sync . s3://$S3_BUCKET_NAME/ --delete
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment to AWS S3 was successful!'
        }
        failure {
            echo '❌ Deployment failed. Check the logs for more information.'
        }
    }
}

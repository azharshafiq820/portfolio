pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
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
                echo 'No build required for static websites.'
            }
        }

        stage('Deploy to AWS S3') {
            steps {
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials']
                ]) {
                    sh '''
                        export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                        export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                        aws s3 sync . s3://$S3_BUCKET --delete
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}

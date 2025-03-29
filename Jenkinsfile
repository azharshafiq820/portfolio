pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_KEY')
        AWS_REGION = 'us-east-1'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install AWS CLI') {
            steps {
                script {
                    def awsCliCheck = sh(script: 'which aws', returnStatus: true)
                    if (awsCliCheck != 0) {
                        echo 'AWS CLI not found. Installing...'
                        sh 'sudo apt-get update && sudo apt-get install awscli -y'
                    } else {
                        echo 'AWS CLI is already installed.'
                    }
                }
            }
        }

        stage('Deploy to S3') {
            steps {
                script {
                    sh '''
                    export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                    export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                    export AWS_REGION=$AWS_REGION

                    aws s3 sync . s3://jenkins7658 --delete
                    echo "Deployment Successful!"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

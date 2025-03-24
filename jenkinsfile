pipeline {
    agent any
    
    environment {
        AWS_REGION = 'us-east-1' // Change to your region
        S3_BUCKET = 'mern-frontend-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AnuragRajput-cyber/Serverless-Application-AWS-.git'
            }
        }
        
        stage('Install Frontend Dependencies') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }
        
        stage('Build Frontend') {
            steps {
                dir('server') {
                    sh 'npm run build'
                }
            }
        }
        
        stage('Deploy Frontend to S3') {
            steps {
                dir('client') {
                    sh 'aws s3 sync build/ s3://${S3_BUCKET} --delete'
                }
            }
        }
        
        stage('Install Backend Dependencies') {
            steps {
                dir('server') {
                    sh 'npm install'
                }
            }
        }
        
        stage('Deploy Backend to Lambda') {
            steps {
                dir('server') {
                    sh 'zip -r lambda.zip .'
                    sh 'aws lambda update-function-code --function-name mern-backend --zip-file fileb://lambda.zip --region ${AWS_REGION}'
                }
            }
        }
    }
    
    post {
        success {
            slackSend(color: 'good', message: "Build Successful: ${env.JOB_NAME} ${env.BUILD_NUMBER}")
        }
        failure {
            slackSend(color: 'danger', message: "Build Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}")
        }
    }
}

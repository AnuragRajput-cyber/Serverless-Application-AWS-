pipeline {
    agent any
    
    // Add this tools section to ensure Node.js is available
    tools {
        nodejs 'Node16' // Must match Node.js installation name in Jenkins
    }
    
    environment {
        AWS_REGION = 'us-east-1' // Change to your region
        S3_BUCKET = 'mern-frontend-app'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM', 
                         branches: [[name: '*/main']],
                         extensions: [],
                         userRemoteConfigs: [[url: 'https://github.com/AnuragRajput-cyber/Serverless-Application-AWS-.git']]
                        ])
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
                dir('frontend') {  // Changed from 'server' to 'frontend'
                    sh 'npm run build'
                }
            }
        }
        
        stage('Deploy Frontend to S3') {
            steps {
                dir('frontend') {  // Changed from 'client' to 'frontend'
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
        always {
            echo 'Pipeline execution completed'
            // Removed slackSend since it's causing errors
            // You can add it back after installing Slack plugin
        }
    }
}

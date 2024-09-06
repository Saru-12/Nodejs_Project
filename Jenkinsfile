pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        S3_BUCKET = 'your-s3-bucket'
        APPLICATION_NAME = 'SimpleWebApp'
        DEPLOYMENT_GROUP_NAME = 'WebAppDeploymentGroup'
    }

    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/Saru-12/Nodejs_Project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Package Application') {
            steps {
                sh 'zip -r simple-web-app.zip *'
                sh 'aws s3 cp simple-web-app.zip s3://$S3_BUCKET/'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                aws deploy create-deployment --application-name $APPLICATION_NAME \
                    --deployment-group-name $DEPLOYMENT_GROUP_NAME \
                    --s3-location bucket=$S3_BUCKET,key=simple-web-app.zip,bundleType=zip \
                    --region us-east-1
                '''
            }
        }
    }
}

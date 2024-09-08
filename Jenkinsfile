pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        APPLICATION_NAME = 'NodejsApp'
        DEPLOYMENT_GROUP = 'NodejsApp-DeploymentGroup'
        S3_BUCKET = 'nodejspo'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Saru-12/Nodejs_Project.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('Nodejs_Project') {  // Change to the directory where package.json is located
                    sh 'npm install'
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Build Step'
            }
        }
        stage('Package Application') {
            steps {
                dir('Nodejs_Project') {  // Ensure you're in the correct directory
                    sh 'zip -r NodejsApp.zip *'
                    script {
                        def awsCli = sh(script: 'aws s3 cp NodejsApp.zip s3://$S3_BUCKET/', returnStdout: true)
                        echo awsCli
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def deployCmd = """
                        aws deploy create-deployment --application-name $APPLICATION_NAME --deployment-group-name $DEPLOYMENT_GROUP --s3-location bucket=$S3_BUCKET,key=NodejsApp.zip,bundleType=zip
                    """
                    sh deployCmd
                }
            }
        }
    }
}



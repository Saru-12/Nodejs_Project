pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        APPLICATION_NAME = 'NodejsApp'  // Update with your CodeDeploy application name
        DEPLOYMENT_GROUP = 'NodejsApp-DeploymentGroup'  // Update with your deployment group name
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
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                echo 'Build Step'
                // Add any build steps here if needed, or leave it as a placeholder
            }
        }
        stage('Package Application') {
            steps {
                sh 'zip -r NodejsApp.zip *'
                script {
                    def awsCli = sh(script: 'aws s3 cp NodejsApp.zip s3://$S3_BUCKET/', returnStdout: true)
                    echo awsCli
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


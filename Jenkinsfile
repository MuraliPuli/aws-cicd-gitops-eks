
pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPOSITORY = 'aws-cicd-demo'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t aws-cicd-demo:latest .'
            }
        }

        stage('Login and Push to ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials'
                ]]) {
                    sh '''
                        ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

                        aws ecr get-login-password --region $AWS_REGION |
                        docker login --username AWS --password-stdin \
                          $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com

                        docker tag aws-cicd-demo:latest \
                          $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPOSITORY:latest

                        docker push \
                          $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPOSITORY:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and image publishing stages completed.'
        }

        failure {
            echo 'Pipeline failed. Check the Jenkins console output.'
        }
    }
}

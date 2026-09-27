pipeline {
    agent any

    environment {
        AWS_REGION     = 'us-east-1'
        ECR_REGISTRY   = '312098798838.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'sre-webapp'
    }

    stages {

        stage('Verify Source Code') {
            steps {
                sh '''
                    pwd
                    ls -la
                    ls -la app
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t sre-webapp:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Login to AWS ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region ${AWS_REGION} | \
                    docker login --username AWS \
                    --password-stdin ${ECR_REGISTRY}
                '''
            }
        }

        stage('Tag Docker Image for ECR') {
            steps {
                sh '''
                    docker tag sre-webapp:${BUILD_NUMBER} \
                    ${ECR_REGISTRY}/${ECR_REPOSITORY}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                sh '''
                    docker push \
                    ${ECR_REGISTRY}/${ECR_REPOSITORY}:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo 'Docker image successfully pushed to AWS ECR.'
        }

        failure {
            echo 'Pipeline failed. Check console output.'
        }
    }
}
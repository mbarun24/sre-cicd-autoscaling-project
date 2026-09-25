pipeline {
    agent any

    stages {

        stage('Checkout GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mbarun24/sre-cicd-autoscaling-project.git'
            }
        }

        stage('Verify Source Code') {
            steps {
                sh '''
                    echo "===== Working Directory ====="
                    pwd

                    echo "===== Repository Files ====="
                    ls -la

                    echo "===== Application Files ====="
                    ls -la app
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "===== Building Docker Image ====="
                    docker build -t sre-webapp:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh '''
                    echo "===== Docker Images ====="
                    docker images
                '''
            }
        }
    }
}
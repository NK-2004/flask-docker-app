pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        IMAGE_REPO_NAME = 'flask-docker-ecr'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Code already checked out by Jenkins"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_REPO_NAME .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag $IMAGE_REPO_NAME:latest <your-account-id>.dkr.ecr.$AWS_REGION.amazonaws.com/$IMAGE_REPO_NAME:latest'
            }
        }

        stage('Login to ECR') {
            steps {
                sh 'aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin <your-account-id>.dkr.ecr.$AWS_REGION.amazonaws.com'
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push <your-account-id>.dkr.ecr.$AWS_REGION.amazonaws.com/$IMAGE_REPO_NAME:latest'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}

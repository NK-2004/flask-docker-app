pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        IMAGE_NAME = 'flask-docker-app'
        ECR_REPO_URI = '123456789012.dkr.ecr.ap-south-1.amazonaws.com/flask-docker-app'
        AWS_CREDENTIALS = credentials('0efd64b7-5cc8-4e79-8587-4c1e09c4ed5e')
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/NK-2004/flask-docker-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    def commit = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                    env.IMAGE_TAG = "${IMAGE_NAME}:${commit}"
                    sh "docker tag $IMAGE_NAME $ECR_REPO_URI:${commit}"
                }
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_DEFAULT_REGION \
                    | docker login --username AWS --password-stdin $ECR_REPO_URI
                '''
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push $ECR_REPO_URI:${IMAGE_TAG.split(":")[1]}'
            }
        }
    }
}

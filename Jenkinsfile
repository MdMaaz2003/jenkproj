pipeline {
    agent any

    environment {
        IMAGE_NAME     = "demo-app"
        DOCKERHUB_REPO = "mohdmaaz777/demo-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Package') {
            steps {
                bat '''
                mvn clean package -DskipTests
                '''
            }
        }

        stage('Docker Build') {
            steps {
                bat '''
                docker build -t %IMAGE_NAME%:latest .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry(credentialsId: 'Docker-Cred', url: '') {
                    bat '''
                    docker tag %IMAGE_NAME%:latest %DOCKERHUB_REPO%:latest
                    docker push %DOCKERHUB_REPO%:latest
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                bat '''
                docker compose down || exit 0
                docker compose up -d --build
                '''
            }
        }
    }

    post {
        success {
            echo "Application built, pushed, and deployed successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}

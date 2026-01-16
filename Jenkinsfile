pipeline {
    agent any

    stages {

        stage('Git-Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/MdMaaz2003/jenkproj.git'
            }
        }

        stage('Install') {
            steps {
                bat 'npm install'
            }
        }

        stage('Trivy Scan') {
            steps {
                bat '''
                trivy fs "%WORKSPACE%" --format table -o trivy-filescan.txt
                '''
            }
        }

        stage('Docker Build & Push') {
            steps {
                withDockerRegistry(credentialsId: 'Docker-Cred', url: '') {
                    bat '''
                    docker build -t mohdmaaz777/my-node-app .
                    docker push mohdmaaz777/my-node-app
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            input {
                message "Approve Deployment to PROD?"
                ok "Deploy"
            }
            steps {
                bat '''
                docker compose down || exit 0
                docker compose up -d
                '''
            }
        }
    }
}

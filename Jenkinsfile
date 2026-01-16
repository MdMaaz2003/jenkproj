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
            steps {
                bat '''
                docker compose down || exit 0
                docker compose up -d --build
                '''
            }
        }
    }
}

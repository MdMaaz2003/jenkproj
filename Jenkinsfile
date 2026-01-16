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
                dir
                '''
            }
        }

        stage('Docker Build & Push') {
            steps {
                withDockerRegistry(credentialsId: 'Docker-Cred', url: '') {
                    bat '''
                    docker build -t mohdmaaz777/nodeproject .
                    docker push mohdmaaz777/nodeproject
                    '''
                }
            }
        }

        stage('Deploy') {
            input {
                message "Approve Deployment to PROD?"
                ok "Deploy"
            }
            steps {
                bat '''
                docker stop node-app || exit 0
                docker rm node-app || exit 0
                docker run -d -p 3000:3000 --name node-app mohdmaaz777/nodeproject
                '''
            }
        }
    }
}


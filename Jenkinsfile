pipeline {
    agent any
    environment {
        DOCKER_USERNAME = credentials('docker_username') // Jenkins credentials ID
        DOCKER_PASSWORD = credentials('docker-password') // Jenkins credentials ID
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the 'main' branch
                git url: 'https://github.com/JashodaKumawat123/Nodeapp.git', branch: 'main'
            }
        }
        stage('Docker Login') {
            steps {
                bat """
                    echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
                """
            }
        }
        stage('Build Docker Image') {
            steps {
                bat """
                    docker build -t %DOCKER_USERNAME%/node-app:latest .
                """
            }
        }
        stage('Push Docker Image') {
            steps {
                bat """
                    docker push %DOCKER_USERNAME%/node-app:latest
                """
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed. Cleaning up if necessary.'
        }
    }
}

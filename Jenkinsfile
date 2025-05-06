pipeline {
    agent any

    environment {
        DOCKER_USERNAME = credentials('docker_username') // ID of DockerHub username credential
        DOCKER_PASSWORD = credentials('docker-password') // ID of DockerHub password/token credential
        IMAGE_NAME = 'nodeapp' // Optional: customize the image name
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/JashodaKumawat123/Nodeapp.git'
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
                    docker build -t %DOCKER_USERNAME%/%IMAGE_NAME%:latest .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                bat """
                    docker push %DOCKER_USERNAME%/%IMAGE_NAME%:latest
                """
            }
        }

        stage('Deploy Container') {
            steps {
                bat """
                    docker stop node-container || exit 0
                    docker rm node-container || exit 0
                    docker run -d -p 3000:3000 --name node-container %DOCKER_USERNAME%/%IMAGE_NAME%:latest
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

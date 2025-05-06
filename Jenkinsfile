pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'yourdockerhubusername/node-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your/repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    script {
                        docker.image("${DOCKER_IMAGE}").push('latest')
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name node-container ${DOCKER_IMAGE}'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker stop node-container || true'
            sh 'docker rm node-container || true'
        }
    }
}

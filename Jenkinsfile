pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sharadhiram/myapp1"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                // Changed 'sh' to 'bat' for Windows
                bat "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    // Changed 'sh' to 'bat' and updated the echo syntax for Windows
                    bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat "docker push ${DOCKER_IMAGE}:latest"
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Image built and pushed to Docker Hub!'
        }
        failure {
            echo 'FAILED: Check the console output for errors.'
        }
    }
}
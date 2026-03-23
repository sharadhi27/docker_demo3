

pipeline {
    agent any

    environment {
        // Ensure this matches your Docker Hub username/repository
        DOCKER_IMAGE = "sharadhiram/myapp1"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                // Using 'sh' (shell) to run standard Docker commands
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                // This pulls the 'dockerhub-creds' you created in Jenkins
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    // Logs in using the environment variables from credentials
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Pushes the finished image to your Hub
                sh "docker push ${DOCKER_IMAGE}:latest"
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
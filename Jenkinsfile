pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        CONTAINER_NAME = "myapp-container"
    }

    stages {

        stage('Remove Old Image') {
            steps {
                sh '''
                docker rmi myapp:latest || true
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t myapp:latest .
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                docker stop myapp-container || true
                docker rm myapp-container || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d \
                --name myapp-container \
                -p 8081:80 \
                myapp:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
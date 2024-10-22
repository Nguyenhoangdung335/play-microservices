pipeline {
    agent any

    environment {
        // Define any environment variables needed
        DOCKER_IMAGE = 'your-docker-image-name'
    }

    stages {
        stage('Build') {
            steps {
                sh 'echo Bulding...'
            }
        }
        stage('Test') {
            steps {
                sh 'echo Testing...'
            }
        }
        stage('Deploy') {
            steps {
                // Deploy the Docker container on the server
                sh '''
                echo Deploying...
                '''
            }
        }
    }
}

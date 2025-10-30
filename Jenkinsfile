pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'kavyasrimandala/node-app'
        DOCKER_CREDENTIALS = 'dockerhub'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                git branch: 'main', url: 'https://github.com/your-repo/node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo 'Logging in to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: "$DOCKER_CREDENTIALS", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Cleanup') {
            steps {
                echo 'Removing local Docker image...'
                sh 'docker rmi $DOCKER_IMAGE:latest || true'
            }
        }
    }

    post {
        success {
            echo '✅ Docker image built and pushed successfully!'
        }
        failure {
            echo '❌ Build failed. Check the logs for details.'
        }
    }
}

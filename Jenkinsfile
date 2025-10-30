pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('kavyasrimandala')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kavyasrivenkat/node-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t kavyasrimandala/node-app .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push kavyasrimandala/node-app'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker logout'
            }
        }
    }
}

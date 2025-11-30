pipeline {
    agent any

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
                withCredentials([usernamePassword(credentialsId: 'kavyasrimandala',
                                                  usernameVariable: 'DOCKERHUB_USER',
                                                  passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh '''
                        echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
                    '''
                }
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
        stage('Deploy to Kubernetes') {
    steps {
        withCredentials([file(credentialsId: 'kubeconfig-cred', variable: 'KUBECONFIG')]) {
            sh '''
                export KUBECONFIG=$KUBECONFIG
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                kubectl rollout status deployment/node-app
            '''
        }
    }
}

    }
}

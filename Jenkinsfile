pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t jenkins-demo .'
            }
        }

        stage('Deploy Container') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker rm -f demo-web || true

                    docker run -d \
                      --name demo-web \
                      -p 8081:80 \
                      jenkins-demo
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
        }
    }
}
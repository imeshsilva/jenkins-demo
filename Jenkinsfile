pipeline {
    agent any

    parameters {

    choice(
        name: 'ENVIRONMENT',
        choices: ['development', 'production'],
        description: 'Select deployment environment'
    )

}

    environment {
        IMAGE_NAME = 'jenkins-demo'
         CONTAINER_NAME = 'demo-web'
         PORT = '8081'
    }

    stages {

        

        stage('Build Docker Image') {
            steps {

                 echo "Deploying to ${params.ENVIRONMENT}"

                echo 'Building Docker image...'
                sh '''
docker build -t $IMAGE_NAME .
'''
            }
        }

        stage('STOP Existing Container') {
            steps {
                echo 'Stopping existing container...'

                sh '''
docker rm -f $CONTAINER_NAME || true
'''
            }
        }

        stage('RUN Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh '''
docker run -d \
  --name $CONTAINER_NAME \
  -p $PORT:80 \
  $IMAGE_NAME
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
def containerName = ""
def port = ""

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
}

    stages {
        stage('Configure Deployment') {
    steps {
        script {
    if (params.ENVIRONMENT == "development") {
        containerName = "demo-web-dev"
        port = "8081"
    } else {
        containerName = "demo-web-prod"
        port = "8082"
    }

    echo "Environment: ${params.ENVIRONMENT}"
    echo "Container: ${containerName}"
    echo "Port: ${port}"
}
    }
}

        

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

                sh """
docker rm -f ${containerName} || true
"""
            }
        }
        

        stage('RUN Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh """
docker run -d \
  --name ${containerName} \
  -p ${port}:80 \
  \$IMAGE_NAME
"""
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
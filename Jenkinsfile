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
        IMAGENAME = 'jenkins-demo'
        PORT = '8081'
        DOCKERNAME = 'demo-web'
    }

    stages {

        

        stage('Build Docker Image') {
            steps {

                 echo "Deploying to ${params.ENVIRONMENT}"

                echo 'Building Docker image...'
                sh "docker build -t ${IMAGENAME} ."
            }
        }

        stage('STOP Existing Container') {
            steps {
                echo 'Stopping existing container...'

                sh "
                    docker rm -f ${DOCKERNAME} || true

                "
            }
        }

        stage('RUN Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh "
                    docker run -d \
                      --name ${DOCKERNAME} \
                      -p ${PORT}:80 \
                      ${IMAGENAME}
                      
                      "
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
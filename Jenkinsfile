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

  

    stages {
        stage('Configure Deployment') {
    steps {
        script {
    if (params.ENVIRONMENT == "development") {
        containerName = "demo-web"
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
        steps    
            {
            sh """
            docker build \
            -t imeshsilva/jenkins-demo:${env.BUILD_NUMBER} \
            -t imeshsilva/jenkins-demo:latest \
            .
            """
            }
            }

            stage('Push Image') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh """
            echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin

            docker push imeshsilva/jenkins-demo:${env.BUILD_NUMBER}
            docker push imeshsilva/jenkins-demo:latest

            docker logout
            """
        }
    }
}

stage('Deploy'){
    steps{
        sh '''
        docker compose down || true
        docker compose up -d
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
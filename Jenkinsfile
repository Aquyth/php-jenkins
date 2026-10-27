pipeline{
    agent any

    environment {
        IMAGE_NAME = 'Aquyth/php-jenkins'
        CONTAINER_NAME = 'php-jenkins'
        APP_PORT = '9000'
    }
    stages{
        stage('checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Aquyth/php-jenkins.git'
            }
        }
        stage('Build Docker Image'){
            sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .
            '''
        }

        stage('PUSH'){
            steps{
                withCredentials([
                    usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_TOKEN') 
                ])
            }
        }
        stage('Deploy'){
            steps{
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:80 ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }
    }
}
pipeline {
    agent any

    environment {
        IMAGE_NAME = "madhu112234/healthprod"
        TAG = "latest"
        CONTAINER_NAME = "healthcare-app"
        HOST_PORT = "8082"
        CONTAINER_PORT = "8080"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Madhu11234/star-agile-health-care.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-token', variable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u madhu112234 --password-stdin
                        docker push $IMAGE_NAME:$TAG
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d -p $HOST_PORT:$CONTAINER_PORT --name $CONTAINER_NAME $IMAGE_NAME:$TAG
                '''
            }
        }
    }
}

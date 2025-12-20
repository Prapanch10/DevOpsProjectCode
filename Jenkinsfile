pipeline {
    agent any

    environment {
        IMAGE_NAME = "prapanch10/dockerimage"
        CONTAINER  = "go-app"
        TAG        = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build --no-cache -t $IMAGE_NAME:$TAG .
                docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:latest
                '''
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    sh '''
                    docker push $IMAGE_NAME:$TAG
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop $CONTAINER || true
                docker rm $CONTAINER || true
                docker run -d -p 8081:8080 --name $CONTAINER $IMAGE_NAME:$TAG
                '''
            }
        }
    }
}

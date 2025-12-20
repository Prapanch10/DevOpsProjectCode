pipeline {
    agent any

    environment {
        IMAGE = "prapanch10/dockerimage:latest"
        CONTAINER = "go-app"
    }

    stages {

        stage('Pull Image from Docker Hub') {
            steps {
                sh 'docker pull $IMAGE'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop $CONTAINER || true
                docker rm $CONTAINER || true
                docker run -d -p 8081:8080 --name $CONTAINER $IMAGE
                '''
            }
        }
    }
}

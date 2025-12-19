pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "prapanch10/dockerimage:latest"
    }

    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop go-app || true
                docker rm go-app || true
                docker run -d -p 8080:8080 --name go-app $DOCKER_IMAGE
                '''
            }
        }
    }
}

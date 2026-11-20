
pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        DOCKER_IMAGE = 'amina47/monapp'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker version'
                bat 'docker build -t %DOCKER_IMAGE%:build-%BUILD_NUMBER% .'
                bat 'docker tag %DOCKER_IMAGE%:build-%BUILD_NUMBER% %DOCKER_IMAGE%:latest'
            }
        }

        stage('Smoke Test') {
            steps {
                bat 'docker run -d -p 8083:80 --name monapp-smoke %DOCKER_IMAGE%:latest'
                bat 'curl -f http://localhost:8083'
                bat 'docker stop monapp-smoke'
                bat 'docker rm monapp-smoke'
            }
        }

        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    bat 'echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USER% --password-stdin'
                    bat 'docker push %DOCKER_IMAGE%:build-%BUILD_NUMBER%'
                    bat 'docker push %DOCKER_IMAGE%:latest'
                    bat 'docker logout'
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        IMAGE_NAME = "amit9166/jenkins-demo-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/amit9166/jenkinsProject1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                bat 'docker push %IMAGE_NAME%:%IMAGE_TAG%'
            }
        }

    }

    post {
        success {
            echo 'Docker Image Pushed Successfully'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}
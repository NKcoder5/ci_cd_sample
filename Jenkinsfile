pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nkcoder5/sample-node-ci"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/NKcoder5/ci_cd_sample.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    bat "echo %PASS% | docker login -u nkcoder5 --password-stdin"
                    bat "docker push %DOCKER_IMAGE%:latest"
                }
            }
        }
    }

    post {
        success {
            slackSend channel: '#ci-cd', message: '✔ Jenkins Build Success 🚀'
        }
        failure {
            slackSend channel: '#ci-cd', message: '❌ Jenkins Build Failed'
        }
    }
}

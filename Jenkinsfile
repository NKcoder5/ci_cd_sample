pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nkcoder5/sample-node-ci"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NKcoder5/ci_cd_sample.git'
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
                bat "docker build -t ${env.DOCKER_IMAGE}:latest ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    bat "echo ${env.PASS} | docker login -u nkcoder5 --password-stdin"
                    bat "docker push ${env.DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        success {
            slackSend(channel: '#ci-cd', tokenCredentialId: 'slack-token', message: '✔ Jenkins Build Success 🚀')
        }
        failure {
            slackSend(channel: '#ci-cd', tokenCredentialId: 'slack-token', message: '❌ Jenkins Build Failed')
        }
    }
}

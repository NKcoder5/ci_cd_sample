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
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${env.DOCKER_IMAGE}:latest ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh "echo ${env.PASS} | docker login -u nkcoder5 --password-stdin"
                    sh "docker push ${env.DOCKER_IMAGE}:latest"
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

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nkcoder5/sample-node-ci"
    }

    tools {
        // If you have NodeJS Plugin configured in Jenkins, you can uncomment this:
        // nodejs 'node-20'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NKcoder5/ci_cd_sample.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Linux shell command
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
            // Make sure you have a Slack Secret Text credential with ID 'slack-token'
            slackSend(channel: '#ci-cd', tokenCredentialId: 'slack-token', message: '✔ Jenkins Build Success 🚀')
        }
        failure {
            slackSend(channel: '#ci-cd', tokenCredentialId: 'slack-token', message: '❌ Jenkins Build Failed')
        }
    }
}

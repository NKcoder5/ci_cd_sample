pipeline {
    agent {
        docker {
            image 'node:20'      // Node 20 official Docker image
            args '-u root:root'  // Run as root so npm install works
        }
    }

    environment {
        DOCKER_IMAGE = "nkcoder5/sample-node-ci"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/NKcoder5/ci_cd_sample.git', branch: 'main'
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
                sh "docker build -t $DOCKER_IMAGE:latest ."
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh "echo $PASS | docker login -u nkcoder5 --password-stdin"
                    sh "docker push $DOCKER_IMAGE:latest"
                }
            }
        }
    }

    post {
        success {
            echo '✔ Build Successful!'
        }
        failure {
            echo '❌ Build Failed!'
        }
    }
}

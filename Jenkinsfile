pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/amlan2601/automation.git', branch: 'master'
            }
        }
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build("mdamlan139/hello-world:${env.BUILD_ID}")
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker run -d -p 80:5000 mdamlan139/hello-world:latest'
                }
            }
        }
    }
}


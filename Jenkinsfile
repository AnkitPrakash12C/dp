pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ankitprakash12c/dp:latest'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/AnkitPrakash12C/dp'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubCreds', usernameVariable: 'ankitprakash12c', passwordVariable: 'AnkitPrakash12C')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose down || true'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }

    options {
        disableConcurrentBuilds()
    }
}

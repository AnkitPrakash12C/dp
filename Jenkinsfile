pipeline {
    agent any

    environment {
        IMAGE_TAG = "1.0.${BUILD_NUMBER}"
        IMAGE_NAME = "ankitprakash1808/notes-app:${IMAGE_TAG}"
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
                    docker.image('docker').inside {
                        sh "docker build -t ${IMAGE_NAME} ."
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerHubCreds') {
                        docker.image("${IMAGE_NAME}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.image('docker/compose:1.29.2').inside('-v $PWD:/app -w /app') {
                        sh 'docker-compose down || true'
                        sh 'docker-compose up -d'
                    }
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

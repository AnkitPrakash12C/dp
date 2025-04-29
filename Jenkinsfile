pipeline {
    agent any

    environment {
        IMAGE_NAME = 'arshah22/notes-app:latest'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/AdeyRatnaShah/NotesApp'
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

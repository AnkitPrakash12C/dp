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

        stage('Clean Up Old Containers') {
            steps {
                script {
                    sh '''
                        echo "Stopping and removing old containers..."
                        docker stop db_cont || true
                        docker rm -f db_cont || true

                        docker stop django_cont || true
                        docker rm -f django_cont || true

                        docker-compose down || true
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
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

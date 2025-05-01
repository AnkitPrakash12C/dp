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

        stage('Deploy') {
            steps {
                script {
                    // Remove any existing containers before deployment
                    sh '''
                        docker-compose down || true
                        docker ps -aq --filter "name=django_cont" | xargs -r docker rm -f
                        docker ps -aq --filter "name=db_cont" | xargs -r docker rm -f
                        docker-compose up -d
                    '''
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

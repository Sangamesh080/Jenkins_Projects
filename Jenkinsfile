pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "python-app"
        DOCKERHUB_CREDENTIALS = "dockerhub"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pytest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('',DOCKERHUB_CREDENTIALS) {
                    docker.image(DOCKER_IMAGE).push("latest")
                    }
                }             
            }
        }
    }

    post {
    always {
        sh 'docker system prune -f'
        }
    }
}


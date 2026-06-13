pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = "$docker_cred"
    }

stages {
    stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t sangamesh080/pyapp1 .'
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
                    docker.image(sangamesh080/pyapp1).push("latest")
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


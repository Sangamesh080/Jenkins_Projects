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

       stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('',DOCKERHUB_CREDENTIALS) {
                        sh 'docker push sangamesh080/pyapp1:01'
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


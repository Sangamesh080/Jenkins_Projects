pipeline {
    agent any

    environment {
        REGISTRY_REPO    = 'sangamesh080/pyapp1'
        CREDENTIALS_ID   = 'docker_cred'
        IMAGE_TAG        = "${env.BUILD_NUMBER}"
    }

stages {
    stage('Build Docker Image') {
            steps {
                script {
                      // Builds the image and tags it with the Jenkins build number
                    sh "docker build -t ${REGISTRY_REPO}:${IMAGE_TAG} ."
                    // Optional: Tag as latest for tracking production release
                    sh "docker build -t ${REGISTRY_REPO}:latest ."
                }
            }
        }

       stage('Push to Docker Hub') {
            steps {
                script {
                     withCredentials([usernamePassword(credentialsId: "${CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        // Log in to Docker Hub via standard input
                        sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                        
                        // Push both the build-numbered tag and the latest tag
                        sh "docker push ${REGISTRY_REPO}:${IMAGE_TAG}"
                        sh "docker push ${REGISTRY_REPO}:latest"
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


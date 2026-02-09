pipeline {
    agent any

    environment {
        IMAGE_NAME = "user0511/myapp"
        IMAGE_TAG  = "v1"
        DOCKERHUB_CREDS = "docker"
    }

    stages {

        stage("Checkout Code") {
            steps {
                checkout scm
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage("Docker Hub Login") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKERHUB_CREDS,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                }
            }
        }

        stage("Push Image to Docker Hub") {
            steps {
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo "✅ Image successfully pushed to Docker Hub"
        }
        failure {
            echo "❌ Pipeline failed"
        }
    }
}

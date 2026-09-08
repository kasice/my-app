pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker.io/kasice/my-app:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                // Jenkins automatically clones your repo when using Pipeline from SCM
                echo "Repository checked out from GitHub"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    echo "Building Docker image: $IMAGE_NAME"
                    docker build -t $IMAGE_NAME .
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub_cred',
                                                  usernameVariable: 'DOCKERHUB_USER',
                                                  passwordVariable: 'DOCKERHUB_PASS')]) {
                    sh """
                        echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                    """
                }
            }
        }

        stage('Push Image') {
            steps {
                sh """
                    echo "Pushing Docker image to Docker Hub"
                    docker push $IMAGE_NAME
                """
            }
        }
    }

    post {
        success {
            echo "Build and push completed successfully!"
        }
        failure {
            echo "Build failed — check console output."
        }
    }
}

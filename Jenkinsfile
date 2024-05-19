pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    // Checkout source code
                    checkout scm

                    // Build Docker image
                    sh 'docker build -t myapp .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests in Docker image
                    sh 'docker run myapp pytest'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Login to Docker registry
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'

                        // Tag and push Docker image to registry
                        sh 'docker tag myapp:latest myregistry/myapp:latest'
                        sh 'docker push myregistry/myapp:latest'
                    }
                }
            }
        }
    }
}

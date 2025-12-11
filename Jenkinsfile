pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'epiking4/jenkins2'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning or updating repository..."
                bat '''
                if not exist ".git" (
                    git clone https://github.com/epiking4/Jenkins2.git .
                ) else (
                    echo Repository exists. Fetching latest...
                    git fetch --all
                    git reset --hard origin/main
                )
                '''
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                bat 'dir' // Replace with your actual build command if needed
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Add actual test commands here, e.g., `bat 'npm test'`
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    echo "Building and pushing Docker image..."
                    docker.withServer('tcp://localhost:2375') {
                        def dockerImage = docker.build("${DOCKER_IMAGE}:latest", "--no-cache .")
                        docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                            dockerImage.push('latest')
                        }
                    }
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                echo "Deploying Docker container locally..."
                bat """
                    docker stop Jenkins2 || echo Container not running
                    docker rm -f Jenkins2 || echo No container to remove
                    docker run -d --name Jenkins2 -p 8090:3000 ${DOCKER_IMAGE}:latest
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}

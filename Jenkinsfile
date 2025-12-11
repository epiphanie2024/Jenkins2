pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo "Building the project..."
                bat 'dir'   // Correct command for Windows
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                bat 'echo Tests executed successfully'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying..."
                bat 'echo Deployment completed'
            }
        }
    }
}

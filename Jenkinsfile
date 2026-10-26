pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Starting Jenkins CI/CD practical'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the logs!'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}

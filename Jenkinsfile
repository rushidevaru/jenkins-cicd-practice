
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking files downloaded from GitHub'
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application - webhook test'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application'

                withCredentials([
                    string(
                        credentialsId: 'demo-secret',
                        variable: 'MY_SECRET'
                    )
                ]) {
                    sh '''
                        if [ -n "$MY_SECRET" ]; then
                            echo "Credential loaded successfully"
                        else
                            echo "Credential is empty"
                            exit 1
                        fi
                    '''
                }
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


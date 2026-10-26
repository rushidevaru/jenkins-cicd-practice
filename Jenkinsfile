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

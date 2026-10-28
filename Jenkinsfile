
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out website code from GitHub'
                sh 'pwd'                     // Show the Jenkins workspace
                sh 'ls -la'                  // List downloaded project files
            }
        }

        stage('Build') {
            steps {
                echo 'Preparing website for deployment'
                sh 'test -f index.html'      // Confirm the website file exists
            }
        }

        stage('Deploy Website to Nginx') {
            steps {
                echo 'Deploying website to target EC2'

                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'demo-ssh-key',
                        keyFileVariable: 'SSH_KEY',
                        usernameVariable: 'SSH_USER'
                    )
                ]) {
                    sh '''
                        set -e

                        scp -i "$SSH_KEY" \
                            -o StrictHostKeyChecking=accept-new \
                            index.html \
                            "$SSH_USER@3.109.123.226:/tmp/index.html"

                        ssh -i "$SSH_KEY" \
                            -o StrictHostKeyChecking=accept-new \
                            "$SSH_USER@3.109.123.226" \
                            'sudo install -o root -g root -m 644 /tmp/index.html /var/www/html/index.html && rm /tmp/index.html'
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Website deployment completed successfully!'
        }

        failure {
            echo 'Deployment failed. Check the Console Output.'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}


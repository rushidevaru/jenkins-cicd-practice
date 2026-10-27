
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

        stage('Test Credentials') {
            steps {
                echo 'Testing Secret Text credential'

                withCredentials([
                    string(
                        credentialsId: 'demo-secret',
                        variable: 'MY_SECRET'
                    )
                ]) {
                    sh '''
                        if [ -n "$MY_SECRET" ]; then
                            echo "Secret text credential loaded successfully"
                        else
                            echo "Secret text credential is empty"
                            exit 1
                        fi
                    '''
                }

                echo 'Testing Username with Password credential'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'demo-username-password',
                        usernameVariable: 'DEMO_USERNAME',
                        passwordVariable: 'DEMO_PASSWORD'
                    )
                ]) {
                    sh '''
                        if [ -n "$DEMO_USERNAME" ] && [ -n "$DEMO_PASSWORD" ]; then
                            echo "Username and password credentials loaded successfully"
                        else
                            echo "Username or password is empty"
                            exit 1
                        fi
                    '''
                }
            }
        }

        stage('SSH Connection Test') {
            steps {
                echo 'Testing SSH connection using Jenkins credentials'

                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'demo-ssh-key',
                        keyFileVariable: 'SSH_KEY',
                        usernameVariable: 'SSH_USER'
                    )
                ]) {
                    sh '''
                        ssh \
                            -o BatchMode=yes \
                            -o StrictHostKeyChecking=accept-new \
                            -i "$SSH_KEY" \
                            "$SSH_USER@localhost" \
                            'whoami && hostname'
                    '''
                }
            }
        }

        stage('Connect to Target EC2') {
            steps {
                echo 'Connecting to the second EC2 instance'

                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'demo-ssh-key',
                        keyFileVariable: 'SSH_KEY',
                        usernameVariable: 'SSH_USER'
                    )
                ]) {
                    sh '''
                        ssh \
                            -o BatchMode=yes \
                            -o ConnectTimeout=10 \
                            -o StrictHostKeyChecking=accept-new \
                            -i "$SSH_KEY" \
                            "$SSH_USER@15.252.242.234" \
                            'echo "SSH connection successful"; whoami; hostname'
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


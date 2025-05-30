pipeline {
    agent any

    environment {
        DEPLOY_USER = 'ubuntu' // or another SSH user
        DEPLOY_HOST = '3.6.36.20'
        DEPLOY_PATH = '/var/www/html/'
        SSH_CREDENTIALS_ID = 'my-ec2-cread-id'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Govindgiri3438/online-shopping-system-php.git', branch: 'main', credentialsId: 'GIT_CREAD'

            }
        }
        

        stage('Deploy') {
            steps {
                sshagent (credentials: [env.SSH_CREDENTIALS_ID]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$DEPLOY_HOST '
                        sudo rm -rf $DEPLOY_PATH/*
                    '
                    scp -o StrictHostKeyChecking=no -r * $DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

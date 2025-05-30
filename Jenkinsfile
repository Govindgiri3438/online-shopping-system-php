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
                git url: 'https://github.com/Govindgiri3438/online-shopping-system-php.git', branch: 'master', credentialsId: 'GIT_CREAD'

            }
        }
        

        stage('Deploy') {
            steps {
                sshagent (credentials: [env.SSH_CREDENTIALS_ID]) {
                    sh """
                    ssh -tt -o StrictHostKeyChecking=no  $DEPLOY_USER@$DEPLOY_HOST '
                        sudo find /var/www/html -mindepth 1 -maxdepth 1 ! -name upload -exec rm -rf {} +
                     '
                        scp -o StrictHostKeyChecking=no -r * $DEPLOY_USER@$DEPLOY_HOST:/tmp/app/
                        sudo cp -r /tmp/app/* /var/www/html/
                        #rsync -av --exclude='upload1/' --exclude='upload2/' /tmp/app/* /var/www/html/
                        sudo chown -R www-data:www-data /var/www/html/
                        sudo chmod -R 755 /var/www/html/
                        sudo systemctl restart apache2
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

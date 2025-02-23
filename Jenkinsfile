pipeline {
    agent any

    environment {
        EC2_IP = '3.7.69.38'
        DEPLOY_DIR = '/var/www/html'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    sshagent(['ec2-ssh-key']) {
                        sh """
                            scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/NginxDeploy/index.html ec2-user@${EC2_IP}:${DEPLOY_DIR}/
                        """
                        sh """
                            ssh -o StrictHostKeyChecking=no ec2-user@${EC2_IP} 'sudo systemctl restart nginx'
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed. Check logs.'
        }
    }
}

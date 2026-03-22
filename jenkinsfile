pipeline {
    agent any

    environment {
        DEV_SERVER = "16.171.115.204"
        STG_SERVER = "13.62.227.80"
        PRD_SERVER = "13.53.216.8"
        USER = "ec2-user"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to DEV') {
            when {
                branch 'Dev'
            }
            steps {
                sh """
                scp -o StrictHostKeyChecking=no index.html $USER@$DEV_SERVER:/tmp/
                ssh $USER@$DEV_SERVER 'sudo cp /tmp/index.html /var/www/html/index.html && sudo systemctl restart httpd'
                """
            }
        }

        stage('Deploy to STG') {
            when {
                branch 'Stg'
            }
            steps {
                sh """
                scp -o StrictHostKeyChecking=no index.html $USER@$STG_SERVER:/tmp/
                ssh $USER@$STG_SERVER 'sudo cp /tmp/index.html /var/www/html/index.html && sudo systemctl restart httpd'
                """
            }
        }

        stage('Deploy to PROD') {
            when {
                branch 'main'
            }
            steps {
                sh """
                scp -o StrictHostKeyChecking=no index.html $USER@$PRD_SERVER:/tmp/
                ssh $USER@$PRD_SERVER 'sudo cp /tmp/index.html /var/www/html/index.html && sudo systemctl restart httpd'
                """
            }
        }
    }

    post {
        success {
            echo "Deployment Successful"
        }

        failure {
            echo "Deployment Failed"
        }
    }
}

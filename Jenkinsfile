pipeline {
    agent any

    environment {
        DEV_SERVER = "51.21.180.177"
        STG_SERVER = "13.62.98.248"
        PRD_SERVER = "51.21.169.5"
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
                branch 'dev'
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
                branch 'stg'
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

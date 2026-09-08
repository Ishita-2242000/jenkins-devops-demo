pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Code has been checked out from GitHub'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "Building application..."
                    hostname
                    whoami
                    pwd
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo Tests passed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }

    post {
        success {
            echo 'Pipeline successful'
        }

        failure {
            echo 'Pipeline failed'
        }

        always {
            echo 'Pipeline completed'
        }
    }
}
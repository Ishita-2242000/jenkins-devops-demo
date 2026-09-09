pipeline {
    agent {
        label 'linux'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Code has been checked out from GitHub'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "Building application with progress..."
                    hostname
                    whoami
                    pwd
                '''
            }
        }

        stage('AWS Authentication') {
            steps {
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-devops-lab']
        ]) {
                sh '''
                    echo "Testing AWS authentication..."
                    aws sts get-caller-identity
                '''
        }
    }
}
        stage('Docker Test') {
            steps {
              sh '''
                echo "Docker version:"
                docker --version

                echo "Docker info:"
                docker ps
             '''
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
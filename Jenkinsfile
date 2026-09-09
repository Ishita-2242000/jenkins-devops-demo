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

        stage('AWS Region') {
             steps {
                withCredentials([
                  [$class: 'AmazonWebServicesCredentialsBinding',
                   credentialsId: 'aws-devops-lab']
        ]) 
        {
                sh '''
                    aws configure get region || true
                    aws sts get-caller-identity
                '''
        }
    }
        stage('ECR Login') {
            steps {
                 withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-devops-lab']
             ]) 
            {
                    sh '''
                        echo "Logging in to Amazon ECR..."

                        aws ecr get-login-password --region us-east-1 | \
                        docker login \
                        --username AWS \
                        --password-stdin 444166849624.dkr.ecr.us-east-1.amazonaws.com
                '''
        }
    }
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
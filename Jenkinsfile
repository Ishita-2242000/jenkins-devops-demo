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
                    echo "Building application..."
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

        stage('ECR Login') {
            steps {
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                     credentialsId: 'aws-devops-lab']
                ]) {
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

        stage('Docker Build') {
            steps {
                sh '''
                    echo "Building Docker image..."

                    docker build -t jenkins-devops-demo:latest .

                    echo "Docker image built successfully"

                    docker images
                '''
            }
        }
        stage('SonarQube Code Quality') {
            steps {
                withSonarQubeEnv('SonarQube') {
                        sh '''
                            sonar-scanner \
                            -Dsonar.projectKey=jenkins-devops-demo \
                            -Dsonar.projectName=jenkins-devops-demo \
                            -Dsonar.sources=.
                        '''
                }
            }
        }
        stage('Trivy Security Scan') {
            steps {
              sh '''
                echo "Running Trivy security scan..."

                trivy image \
                    --severity HIGH,CRITICAL \
                    --exit-code 1 \
                    jenkins-devops-demo:latest
                '''
            }
        }


        stage('Push Image to ECR') {
            steps {
                sh '''
                    echo "Tagging Docker image..."

                    docker tag \
                        jenkins-devops-demo:latest \
                        444166849624.dkr.ecr.us-east-1.amazonaws.com/jenkins-devops-demo:latest

                    echo "Pushing Docker image to ECR..."

                    docker push \
                        444166849624.dkr.ecr.us-east-1.amazonaws.com/jenkins-devops-demo:latest
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                input message: 'Approve deployment to production?',
                      ok: 'Deploy'

                echo 'Production deployment approved!'
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
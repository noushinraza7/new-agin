pipeline{
    agent any

    environment {
        IMAGE_NAME = "php-login-app"
        CONTAINER_NAME = "php-login-app"
    }
    stages{
        stage('1. Git Checkout'){
            steps{
                script{
                    echo 'Downloading PHP source code'
                    checkout scm
            
                }
            }
        }
        stage('2. PHP Syntax Test') {
            steps {
                sh '''
                    docker run --rm -v "$WORKSPACE":/app -w /app php:8.4-cli sh -c 'find . -name "*.php" -not -path "./vendor/*" -print0 | xargs -0 -r -n1 php -l'
                '''
            }
        }
        stage('3. Docker build'){
            steps{
                sh '''
                    docker build -t my-php-app .
                '''
            }
        }
        stage('4. Deploy Application') {
            steps {
                sh '''
                    docker stop my-php-container || true
                    docker rm my-php-container || true

                    docker run -d \
                        --name my-php-container \
                        -p 8082:80 \
                        my-php-app
                '''
            }
        }
        stage('5. Verify Deployment') {
            steps {
                sh '''
                    docker ps
                    curl -f http://localhost:8082
                '''
            }
        }
    }
}
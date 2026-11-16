pipeline{
    agent any

    environment{
        IMAGE_NAME = "php-login-app"
        CONTAINER_NAME =  "php-login-app"

    }
    stages{
        stage('1- git checkout'){
            steps{
                script{
                    echo 'downloading php source code'
                    checkout scm
                }
            }
        stage('2- php syntax test'){
            steps{
                sh '''
                    docker run --rm -v "$WORKSPACE":/app -w /app php:8.4-cli sh -c 'find . -name  "*.php" -not -path "./vendor/* -print0 | xargs -0 -r -n1 php -1' 
                '''
                }
            }
        }
        stage('3- docker build'){
            steps{
                sh'''
                    docker compose build web
                '''
            }
        }
        stage('4- deploy application'){
            steps{
                sh'''
                    docker compose up -d
                '''
            }
        }
    }
}
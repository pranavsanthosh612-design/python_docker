
pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t python-django:latest .'
            }
        }

        stage('Deploy Django') {
            steps {
                sh '''
                    docker stop django-app || true
                    docker rm django-app || true

                    docker run -d \
                      --name django-app \
                      --restart unless-stopped \
                      -p 80:5005 \
                      python-django:latest
                '''
            }
        }
    }
}

pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t java-web-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                docker stop java-container || exit 0
                docker rm java-container || exit 0
                docker run -d -p 8080:8080 --name java-container java-web-app:latest
                '''
            }
        }

    }

    post {

        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }

    }
}
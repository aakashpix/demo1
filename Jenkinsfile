pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Build & Test') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('Cleanup') {
            steps {
                bat '''
                docker stop demo-container
                if %ERRORLEVEL% neq 0 echo No running container
                docker rm demo-container
                if %ERRORLEVEL% neq 0 echo No container to remove
                docker rmi demo-app
                if %ERRORLEVEL% neq 0 echo No image to remove
                '''
            }
        }
        stage('Docker Build') {
            steps {
                bat 'docker build -t demo-app .'
            }
        }
        stage('Deploy') {
            steps {
                bat 'docker run -d --name demo-container -p 8084:8083 demo-app'
            }
        }
    }
}

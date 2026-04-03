pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/divanshu3702/docker-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo Build Started'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t my-app .'
            }
        }

    }
}
  
  

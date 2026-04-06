pipeline {
    agent any

    environment {
        SONAR_SERVER = 'sonar-server'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/divanshu3702/docker-demo.git'
            }
        }

        // ✅ Node.js install dependencies
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // ✅ Run app (optional test)
        stage('Run App') {
            steps {
                sh 'node app.js &'
            }
        }

        // ✅ SonarQube Analysis
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONAR_SERVER}") {
                    sh 'sonar-scanner'
                }
            }
        }

        // ✅ Quality Gate
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        // ✅ Docker Build
        stage('Docker Build') {
            steps {
                sh 'docker build -t demo-app .'
            }
        }

    }

    post {
        success {
            echo '✅ Pipeline Success'
        }
        failure {
            echo '❌ Pipeline Failed'
        }
    }
}

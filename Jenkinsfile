pipeline {
    agent any

    tools {
        nodejs 'NodeJS'   // 👈 Jenkins me configured hona chahiye
    }

    environment {
        SONAR_SERVER = 'sonar-server'
    }

    stages {

        // 🔹 Stage 1: Checkout Code
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/divanshu3702/docker-demo.git'
            }
        }

        // 🔹 Stage 2: Install Dependencies
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // 🔹 Stage 3: SonarQube Analysis
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONAR_SERVER}") {
                    sh 'sonar-scanner'
                }
            }
        }

        // 🔹 Stage 4: Quality Gate
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        // 🔹 Stage 5: Docker Build
        stage('Docker Build') {
            steps {
                sh 'docker build -t demo-app .'
            }
        }

        // 🔹 Stage 6: Run Container (Optional)
        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8081:3000 demo-app || true'
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

pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://sonarqube:9000'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/divanshu3702/docker-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''
                    npx sonar-scanner \
                    -Dsonar.projectKey=docker-demo \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=$SONAR_HOST_URL
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t docker-demo .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                docker stop docker-demo || true
                docker rm docker-demo || true
                docker run -d -p 3000:3000 --name docker-demo docker-demo
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline Successful'
        }
        failure {
            echo '❌ Pipeline Failed'
        }
    }
}

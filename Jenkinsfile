pipeline {
    agent any

    tools {
        nodejs 'NodeJS'   // Make sure NodeJS plugin + tool configured
    }

    environment {
        IMAGE_NAME = "docker-demo"
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

        // ✅ SONAR WORKING (NO TOKEN NEEDED - using Jenkins config)
        stage('SonarQube Analysis') {
            steps {
              withSonarQubeEnv('sonar-server') {
    withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
        sh '''
        npx sonar-scanner \
        -Dsonar.projectKey=docker-demo \
        -Dsonar.sources=. \
        -Dsonar.host.url=$SONAR_HOST_URL \
        -Dsonar.login=$SONAR_TOKEN
        '''
                }
            }
        }

        // ✅ QUALITY GATE FIXED
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        // ✅ DOCKER BUILD
        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        // ✅ DOCKER RUN
        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f docker-demo-container || true
                docker run -d -p 3000:3000 --name docker-demo-container $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline SUCCESS'
        }
        failure {
            echo '❌ Pipeline FAILED'
        }
    }
}

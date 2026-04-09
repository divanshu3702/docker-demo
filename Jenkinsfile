pipeline {
    agent any

    stages {

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
                    -Dsonar.token=$SONAR_AUTH_TOKEN
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
                sh 'docker build -t my-app .'
            }
        }

        stage('Docker Run') {
            steps {
                sh 'docker run -d -p 3000:3000 my-app'
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

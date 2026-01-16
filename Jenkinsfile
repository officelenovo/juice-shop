pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = "juice-shop-sast"
        SONAR_HOST_URL = "http://sonarqube:9000"
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Verify Code Exists') {
            steps {
                sh '''
                  echo "PWD:"
                  pwd
                  echo "TypeScript files:"
                  find . -name "*.ts" | head -20
                '''
            }
        }

        stage('SAST - SonarQube Scan') {
            steps {
                sh '''
                  docker run --rm \
                    -v "$PWD:/usr/src" \
                    sonarsource/sonar-scanner-cli \
                    -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                    -Dsonar.sources=app.ts,server.ts,frontend,lib,routes,models,config \
                    -Dsonar.host.url=$SONAR_HOST_URL \
                    -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }
}

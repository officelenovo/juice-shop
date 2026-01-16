pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://host.docker.internal:9000"
        SONAR_PROJECT_KEY = "juice-shop-sast"
        SONAR_TOKEN = credentials('sonarqube-token')
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SAST - SonarQube Scan') {
            steps {
                sh '''
                docker run --rm \
                  -v "$PWD:/usr/src" \
                  sonarsource/sonar-scanner-cli \
                  -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=$SONAR_HOST_URL \
                  -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }
}


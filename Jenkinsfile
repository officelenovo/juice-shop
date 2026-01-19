pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://host.docker.internal:9000"
        SONAR_PROJECT_KEY = "juice-shop-sast"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                  npm install
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                  npm run build || true
                '''
            }
        }

        stage('SAST - SonarQube Scan') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-token')
            }
            steps {
                sh '''
                docker run --rm \
                  --platform=linux/amd64 \
                  -v "$PWD:/usr/src" \
                  -w /usr/src \
                  sonarsource/sonar-scanner-cli \
                  -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                  -Dsonar.sources=src \
                  -Dsonar.exclusions=**/node_modules/**,**/dist/**,**/coverage/** \
                  -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info \
                  -Dsonar.sourceEncoding=UTF-8 \
                  -Dsonar.scm.provider=git \
                  -Dsonar.host.url=${SONAR_HOST_URL} \
                  -Dsonar.login=${SONAR_TOKEN}
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
    }
}

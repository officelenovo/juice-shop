pipeline {
    agent any

    environment {
        SONAR_HOST_URL   = "http://host.docker.internal:9000"
        SONAR_PROJECT_KEY = "juice-shop-sast"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SAST - SonarQube Scan') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        docker run --rm \
                          -v "$PWD:/usr/src" \
                          -w /usr/src \
                          sonarsource/sonar-scanner-cli \
                          -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                          -Dsonar.sources=. \
                          -Dsonar.exclusions=**/node_modules/**,**/dist/**,**/coverage/** \
                          -Dsonar.scm.provider=git \
                          -Dsonar.host.url=$SONAR_HOST_URL \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ SonarQube SAST scan completed successfully"
        }
        failure {
            echo "❌ SonarQube SAST scan failed"
        }
    }
}

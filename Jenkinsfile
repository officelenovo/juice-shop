pipeline {
    agent any

    environment {
        SONAR_HOST_URL    = "http://host.docker.internal:9000"
        SONAR_PROJECT_KEY = "juice-shop-sast"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube SAST Scan') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    echo "Workspace content:"
                    ls -la

                    docker run --rm \
                      -v "$WORKSPACE:/usr/src" \
                      -w /usr/src \
                      sonarsource/sonar-scanner-cli \
                      -Dsonar.projectKey=juice-shop-sast \
                      -Dsonar.sources=. \
                      -Dsonar.exclusions=**/node_modules/**,**/dist/**,**/coverage/** \
                      -Dsonar.host.url=http://host.docker.internal:9000 \
                      -Dsonar.login=$SONAR_TOKEN \
                      -Dsonar.scm.disabled=true
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

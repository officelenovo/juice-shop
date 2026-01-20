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

        stage('Debug Workspace') {
            steps {
                sh '''
                  echo "Workspace content:"
                  ls -la
                '''
            }
        }

        stage('SAST - SonarQube Scan') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                      docker run --rm \
                        -v "$WORKSPACE:/usr/src" \
                        -w /usr/src \
                        sonarsource/sonar-scanner-cli \
                        -Dsonar.projectKey=$SONAR_PROJECT_KEY \
                        -Dsonar.sources=frontend,lib,routes,models \
                        -Dsonar.inclusions=**/*.ts,**/*.js,**/node_modules/**,**/dist/**,**/coverage/** \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_TOKEN \
                        -Dsonar.scm.disabled=true
                    '''
                }
            }
        }
    }
}

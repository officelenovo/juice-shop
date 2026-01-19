pipeline {
    agent any

    environment {
        SONAR_PROJECT_KEY = "juice-shop-sast"
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
                  echo "Workspace:"
                  pwd
                  echo "Sample TypeScript files:"
                  find . -name "*.ts" | head -10
                '''
            }
        }

        stage('SAST - SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                      sonar-scanner \
                        -Dsonar.projectKey=juice-shop-sast \
                        -Dsonar.sources=. \
                        -Dsonar.exclusions=node_modules/**,dist/** \
                        -Dsonar.language=ts
                    '''
                }
            }
        }
    }
}

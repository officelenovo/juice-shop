pipeline {
    agent any

    environment {
        SONAR_HOST_URL    = "http://host.docker.internal:9000"
        SONAR_PROJECT_KEY = "juice-shop-sast"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('SonarQube SAST') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    docker run --rm \
                      -v "$WORKSPACE:/usr/src" \
                      -w /usr/src \
                      sonarsource/sonar-scanner-cli \
                      -Dsonar.projectKey=juice-shop-sast \
                      -Dsonar.projectBaseDir=/usr/src \
                      -Dsonar.sources=. \
                      -Dsonar.exclusions=**/node_modules/**,**/dist/**,**/coverage/**,**/*.spec.ts \
                      -Dsonar.javascript.file.suffixes=.js,.jsx \
                      -Dsonar.typescript.file.suffixes=.ts,.tsx \
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
            echo "✅ SonarQube analysis completed"
        }
        failure {
            echo "❌ SonarQube analysis failed"
        }
    }
}

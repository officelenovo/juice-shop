pipeline {
    agent any

    environment {
        SONAR_HOST_URL    = "http://sonarqube:9000"
        SONAR_PROJECT_KEY = "juice-shop-sast"
        DOCKER_NETWORK    = "devsecops-net"
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
                      --network ${DOCKER_NETWORK} \
                      -v "$WORKSPACE:/usr/src" \
                      -w /usr/src \
                      sonarsource/sonar-scanner-cli \
                      -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                      -Dsonar.projectBaseDir=/usr/src \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=${SONAR_HOST_URL} \
                      -Dsonar.login=${SONAR_TOKEN} \
                      -Dsonar.scm.disabled=true
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "SonarQube analysis completed successfully"
        }
        failure {
            echo "SonarQube analysis failed"
        }
    }
}

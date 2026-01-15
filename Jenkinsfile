pipeline {
  agent any

  environment {
    SONAR_HOST_URL = "http://sonarqube:9000"
    SONAR_PROJECT_KEY = "juice-shop-sast"
    SONAR_TOKEN = credentials('sonar-token')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('SAST - SonarQube') {
      steps {
        sh '''
          docker run --rm \
            --network container:sonarqube \
            -v "$PWD:/usr/src" \
            sonarsource/sonar-scanner-cli \
            -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
            -Dsonar.sources=. \
            -Dsonar.host.url=${SONAR_HOST_URL} \
            -Dsonar.login=${SONAR_TOKEN}
        '''
      }
    }
  }
}


stage('SAST - SonarQube Scan') {
    steps {
        withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
            sh '''
              pwd
              ls -la

              docker run --rm \
                -v "$PWD:/usr/src" \
                -w /usr/src \
                sonarsource/sonar-scanner-cli \
                -Dsonar.projectKey=juice-shop-sast \
                -Dsonar.sources=. \
                -Dsonar.exclusions=**/node_modules/**,**/dist/**,**/coverage/** \
                -Dsonar.branch.name=main \
                -Dsonar.scm.provider=git \
                -Dsonar.host.url=$SONAR_HOST_URL \
                -Dsonar.login=$SONAR_TOKEN
            '''
        }
    }
}

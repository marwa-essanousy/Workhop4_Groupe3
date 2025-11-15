pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://sonarqube-server:9000'
        SONAR_TOKEN    = 'sqa_73817cd26adf945445f16410d6c530a7cad42d2b'   
        SONAR_NETWORK  = 'sonarqube_sonarnet'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/NassimeElkamari/nodejs-sonar.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    sh """
                    docker run --rm \
                      -e SONAR_HOST_URL="${SONAR_HOST_URL}" \
                      -e SONAR_TOKEN="${SONAR_TOKEN}" \
                      -v "\$(pwd):/usr/src" \
                      --network ${SONAR_NETWORK} \
                      sonarsource/sonar-scanner-cli
                    """
                }
            }
        }
    }
}



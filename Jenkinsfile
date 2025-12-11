pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://sonarqube-server:9000'
        SONAR_TOKEN    = 'sqp_88eb03682b0d3a1749d1018618baee70a826b4bf'   // ❗better to use Jenkins credentials later
        SONAR_NETWORK  = 'sonarqube_sonarnet'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/marwa-essanousy/DevSecOps.git'
            }
        }

        stage('Install dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run tests with coverage') {
            steps {
                bat 'set NODE_ENV=test&& npm test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    bat """
docker run --rm -e SONAR_HOST_URL="${SONAR_HOST_URL}" -e SONAR_TOKEN="${SONAR_TOKEN}" -v "%CD%:/usr/src" --network ${SONAR_NETWORK} sonarsource/sonar-scanner-cli
"""
                }
            }
        }
    }
}

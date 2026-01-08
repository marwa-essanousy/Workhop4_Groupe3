pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'https://sonarcloud.io'
        SONAR_TOKEN    = '5a4eaa6052b3d145eebb490f2b5dd5f9e599b0c1'
        SONAR_ORGANIZATION = 'marwa-essanousy'
        SONAR_PROJECT_KEY  = 'marwa-essanousy_devsecops'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/marwa-essanousy/Workhop3_Groupe3.git'
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

        // stage('OWASP Dependency-Check') {
        //     steps {
        //         bat """
        //         docker run --rm ^
        //           -v "%CD%:/src" ^
        //           owasp/dependency-check ^
        //           --scan /src ^
        //           --format HTML ^
        //           --out /src/dependency-check-report
        //         """
        //     }
        // }

        stage('SonarCloud Analysis') {
            steps {
                bat """
                C:\\sonar-scanner\\bin\\sonar-scanner.bat ^
                  -Dsonar.host.url=%SONAR_HOST_URL% ^
                  -Dsonar.token=%SONAR_TOKEN% ^
                  -Dsonar.organization=%SONAR_ORGANIZATION% ^
                  -Dsonar.projectKey=%SONAR_PROJECT_KEY%
                """
            }
        }
       stage('Trivy Scan (JSON Report)') {
            steps {
                bat """
                docker run --rm ^
                -v "%CD%:/project" ^
                aquasec/trivy:latest ^
                image marwa2526/mon-app-react:latest ^
                --severity HIGH,CRITICAL ^
                --format json ^
                --output /project/trivy-report.json
                """
        }
    }

 }
}

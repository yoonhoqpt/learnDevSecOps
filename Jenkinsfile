pipeline {
    agent any

    environment {
        SONARQUBE_SCANNER_HOME = tool 'SonarQubeScanner'
        DEPENDENCY_TRACK_API_KEY = credentials('dependency-track-api-key')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yoonhoqpt/learnDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Static Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${SONARQUBE_SCANNER_HOME}/bin/sonar-scanner"
                }
            }
        }

        stage('Dependency Analysis - Dependency-Track') {
            steps {
                sh """
                   curl -X POST -H 'Content-Type: application/json' -H 'X-Api-Key: ${DEPENDENCY_TRACK_API_KEY}' \\
                   -d '{ "project": "your-project-uuid", "bom": "$(cat bom.xml)" }' \\
                   http://dependency-track-server/api/v1/bom
                   """
            }
        }
    }
}
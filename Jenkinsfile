pipeline {
    agent any

    environment {
        SONARQUBE_SCANNER_HOME = tool 'SonarQubeScanner'
        DEPENDENCY_TRACK_API_KEY = credentials('dependency-track-api-key')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/react-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
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

        // stage('Container Scan - Trivy') {
        //     steps {
        //         sh 'docker build -t react-app .'
        //         sh 'trivy image --severity HIGH,CRITICAL react-app'
        //     }
        // }

        // stage('Secret Detection') {
        //     steps {
        //         sh 'npm install -g @github/security-detection'
        //         sh 'gh secret scan --repo your-repo/react-app'
        //     }
        // }

        // stage('Dynamic Analysis - OWASP ZAP') {
        //     steps {
        //         sh 'zap-cli start'
        //         sh 'zap-cli status --timeout 300'
        //         sh 'zap-cli spider http://localhost:3000'
        //         sh 'zap-cli active-scan --scanners xss,sqli,http'
        //         sh 'zap-cli report -o zap_report.html'
        //     }
        // }

        // stage('Compliance Checks - Chef InSpec') {
        //     steps {
        //         sh 'inspec exec compliance-profile'
        //     }
        // }
    }

    // post {
    //     always {
    //         archiveArtifacts artifacts: '**/zap_report.html', allowEmptyArchive: true
    //         junit 'reports/**/*.xml'
    //         cleanWs()
    //     }
    // }
}
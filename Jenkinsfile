pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://localhost:9000'
        // DEPENDENCY_TRACK_API_KEY = credentials('dependency-track-api-key')
    }

    stages {
        
        stage('Clean Workspace') {
            steps {
                deleteDir() // Clean the workspace
            }
        }
        
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yoonhoqpt/learnDevSecOps.git'
            }
        }

        // stage('Install Dependencies') {
        //     steps {
        //         sh 'npm install'
        //     }
        // }

        stage('Static Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=my_project -Dsonar.sources=./src -Dsonar.host.url=${SONARQUBE_URL}'
                }
            }
        }

        // stage('Dependency Analysis - Dependency-Track') {
        //     steps {
        //         sh """
        //            curl -X POST -H 'Content-Type: application/json' -H 'X-Api-Key: ${DEPENDENCY_TRACK_API_KEY}' \\
        //            -d '{ "project": "your-project-uuid", "bom": "$(cat bom.xml)" }' \\
        //            http://dependency-track-server/api/v1/bom
        //            """
        //     }
        // }
    }
}
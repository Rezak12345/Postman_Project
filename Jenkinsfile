pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Rezak12345/Postman_Project.git'
            }
        }

        stage('Install dependencies') {
            steps {
                bat '''
                npm install -g newman
                npm install -g newman-reporter-htmlextra
                '''
            }
        }

        stage('Run API Tests') {
            steps {
                bat '''
                chcp 65001
                newman run collections/Collection_Client.postman_collection.json ^
                --environment environments/test.postman_environment.json ^
                --reporters cli,htmlextra ^
                --reporter-htmlextra-export report.html
                '''
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'report.html',
                reportName: 'Newman API Report'
            ])
        }

        failure {
            echo '❌ Tests API échoués'
        }

        success {
            echo '✅ Tests API réussis'
        }
    }
}

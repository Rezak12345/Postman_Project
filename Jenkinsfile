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
                if not exist reports mkdir reports

                newman run Collection_Client.postman_collection.json ^
                -r htmlextra,junit ^
                --reporter-htmlextra-export reports/report.html ^
                --reporter-junit-export reports/report.xml
                '''
            }
        }
    }

    post {
        always {
            // Rapport HTML visuel
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'reports',
                reportFiles: 'report.html',
                reportName: 'Newman API Report'
            ])

            // Résultats de tests intégrés Jenkins
            junit 'reports/report.xml'
        }

        failure {
            echo '❌ Tests API échoués'
        }

        success {
            echo '✅ Tests API réussis'
        }
    }
}

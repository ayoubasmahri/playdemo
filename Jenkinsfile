pipeline {
    agent any

    environment {
        PLAYWRIGHT_JUNIT_REPORT = 'test-results/results.xml'
        PLAYWRIGHT_HTML_REPORT = 'playwright-report'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Run Playwright Tests') {
            steps {
                script {
                    sh """
                        npx playwright install
                        npx playwright test --reporter=junit,html
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Checking if test results exist...'
            sh 'ls -R test-results'  // Debugging step
            sh 'ls -R playwright-report' // Debugging step
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true, fingerprint: true
            echo 'Archiving test reports...'
            junit 'test-results/results.xml'  // Ensure path matches `playwright.config.ts`
            echo 'Publishing HTML report...'
            publishHTML([
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright Test Report'
            ])
        }
    }
}

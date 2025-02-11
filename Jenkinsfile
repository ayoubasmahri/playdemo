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
                        npx playwright test 
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
             echo 'Publishing HTML report...'
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: false,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright Report',
                reportTitles: '',
                useWrapperFileDirectly: true
            ])
            echo 'Archiving test reports...'
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true, fingerprint: true
  // Ensure path matches `playwright.config.ts`
           
        }
    }
}

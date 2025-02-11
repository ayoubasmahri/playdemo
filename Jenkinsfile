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
            htmlPublisher([
    allowMissingProperties: true,
    reportDir: 'playwright-report',
    reportFiles: 'index.html', // Or your report file name(s)
    csp: 'sandbox allow-scripts allow-same-origin; default-src \'self\'; script-src \'self\' \'unsafe-inline\'; style-src \'self\' \'unsafe-inline\'; img-src \'self\' data:;' // Corrected: Added comma here
])
            echo 'Archiving test reports...'
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true, fingerprint: true
  // Ensure path matches `playwright.config.ts`
           
        }
    }
}

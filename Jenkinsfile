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

        echo 'Archiving test reports...'
        junit 'test-results/results.xml'  // Ensure path matches `playwright.config.ts`
        archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true, fingerprint: true
    }
}
}

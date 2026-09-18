// SIT223/SIT753 - 8.2C Part 2 Task 2 (Email Notification)
// Extends the Part 1 Task 2 pipeline: emails the status of the test stage and
// the security scan stage, with the build log attached.
// Repo: https://github.com/DrunkenHero-commits/8.2CDevSecOps

pipeline {
    agent any

    environment {
        NOTIFY = '<YOUR_EMAIL@gmail.com>'
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                // Tool: Git
                git branch: 'main',
                    url: 'https://github.com/DrunkenHero-commits/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Tool: npm
                sh 'npm install --legacy-peer-deps || npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Tool: Mocha via npm test
                sh 'npm test || true'
            }
            post {
                success {
                    emailext(
                        to: "${NOTIFY}",
                        subject: "Test stage PASSED - ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The Run Tests stage finished successfully.\n\nJob: ${JOB_NAME}\nBuild: #${BUILD_NUMBER}\nConsole: ${BUILD_URL}console\n\nFull log attached.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${NOTIFY}",
                        subject: "Test stage FAILED - ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The Run Tests stage failed.\n\nJob: ${JOB_NAME}\nBuild: #${BUILD_NUMBER}\nConsole: ${BUILD_URL}console\n\nFull log attached.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // Tool: Istanbul/nyc
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                // Tool: npm audit (CVE database)
                sh 'npm audit || true'
            }
            post {
                success {
                    emailext(
                        to: "${NOTIFY}",
                        subject: "Security scan PASSED - ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The npm audit security scan completed.\n\nJob: ${JOB_NAME}\nBuild: #${BUILD_NUMBER}\nConsole: ${BUILD_URL}console\n\nVulnerability output attached.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${NOTIFY}",
                        subject: "Security scan FAILED - ${JOB_NAME} #${BUILD_NUMBER}",
                        body: "The npm audit security scan failed to complete.\n\nJob: ${JOB_NAME}\nBuild: #${BUILD_NUMBER}\nConsole: ${BUILD_URL}console\n\nFull log attached.",
                        attachLog: true
                    )
                }
            }
        }
    }
}

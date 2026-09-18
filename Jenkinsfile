// SIT223/SIT753 - 8.2C Part 1 Task 1
// Mock CI/CD pipeline - prints the task and tool for each stage only.
// Repo: https://github.com/DrunkenHero-commits/8.2CPipeline

pipeline {
    agent any

    triggers {
        // Polls GitHub every 5 minutes, so a new commit starts a build.
        // No webhook required for this task.
        pollSCM('H/5 * * * *')
    }

    stages {

        stage('Build') {
            steps {
                sh 'echo "Task: compile the source and package it into a deployable artifact"'
                sh 'echo "Tool: Maven"'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                sh 'echo "Task: run unit tests on individual classes, then integration tests across modules"'
                sh 'echo "Tools: JUnit for unit tests, Selenium for integration tests"'
            }
        }

        stage('Code Analysis') {
            steps {
                sh 'echo "Task: static analysis of the codebase against agreed coding standards"'
                sh 'echo "Tool: SonarQube"'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'echo "Task: scan source and dependencies for known vulnerabilities and CVEs"'
                sh 'echo "Tool: Snyk"'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'echo "Task: release the packaged build to the staging server"'
                sh 'echo "Tool: AWS CLI deploying to an EC2 staging instance"'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                sh 'echo "Task: verify the application end to end in a production-like environment"'
                sh 'echo "Tool: Postman/Newman against the staging endpoint"'
            }
        }

        stage('Deploy to Production') {
            steps {
                sh 'echo "Task: promote the verified build to the production server"'
                sh 'echo "Tool: AWS CodeDeploy to an EC2 production instance"'
            }
        }
    }

    post {
        success {
            sh 'echo "Pipeline finished - all seven stages completed"'
        }
        failure {
            sh 'echo "Pipeline failed - check the stage log above"'
        }
    }
}

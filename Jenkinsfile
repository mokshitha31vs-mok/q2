pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Obtains source files from the designated Git system repo
                git branch: 'main', url: 'https://github.com/mokshitha31vs-mok/j2.git'
            }
        }
        stage('Parallel Checks') {
            parallel {
                stage('Frontend Execution Check') {
                    steps {
                        bat 'python frontend_check.py'
                    }
                }
                stage('Backend Execution Check') {
                    steps {
                        bat 'python backend_check.py'
                    }
                }
            }
        }
        stage('Archive Reports') {
            steps {
                // Captures both generated output logs as persistent workspace items
                archiveArtifacts artifacts: '*_report.txt', fingerprint: true
            }
        }
    }
}

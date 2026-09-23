pipeline {
    agent any

    // Keep several completed builds and their archived artifacts so that an
    // older build can still be inspected after a newer build finishes.
    options {
        buildDiscarder(logRotator(
            numToKeepStr: '10',
            artifactNumToKeepStr: '10'
        ))
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mokshitha31vs-mok/q2.git'
            }
        }

        stage('Parallel Checks') {
            parallel {
                stage('Frontend Check') {
                    steps {
                        script {
                            if (isUnix()) {
                                sh 'python3 frontend_check.py'
                            } else {
                                bat 'python frontend_check.py'
                            }
                        }
                    }
                }
                stage('Backend Check') {
                    steps {
                        script {
                            if (isUnix()) {
                                sh 'python3 backend_check.py'
                            } else {
                                bat 'python backend_check.py'
                            }
                        }
                    }
                }
            }
        }

        stage('Archive Reports') {
            steps {
                archiveArtifacts artifacts: 'frontend_report.txt,backend_report.txt', fingerprint: true
                echo "Build ${env.BUILD_NUMBER} archived both reports."
                echo "They remain available at ${env.BUILD_URL}artifact/frontend_report.txt and ${env.BUILD_URL}artifact/backend_report.txt."
                echo 'Because artifactNumToKeepStr is 10, a subsequent build does not replace this build''s artifacts.'
                echo 'The two 4-second checks run concurrently: parallel time is approximately 4 seconds (plus startup overhead), versus approximately 8 seconds sequentially.'
            }
        }
    }
}

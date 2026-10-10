
pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Build') {
            steps {
                sh './gradlew clean assemble'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Coverage') {
            steps {
                sh './gradlew jacocoTestReport'

                recordCoverage(
                    tools: [[
                        parser: 'JACOCO',
                        pattern: 'app/build/reports/jacoco/test/jacocoTestReport.xml'
                    ]],
                    sourceCodeRetention: 'EVERY_BUILD'
                )
            }
        }
    }

    post {
        always {
            junit 'app/build/test-results/test/*.xml'

            archiveArtifacts(
                artifacts: 'app/build/reports/jacoco/test/**',
                allowEmptyArchive: true
            )
        }
    }
}

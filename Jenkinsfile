
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

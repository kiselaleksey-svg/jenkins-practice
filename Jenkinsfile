pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Repository: ${env.GIT_URL}"
                echo "Branch: ${env.GIT_BRANCH}"
            }
        }

        stage('Create file') {
            steps {
                writeFile(
                    file: 'result.txt',
                    text: 'This text is intentionally incorrect'
                )
            }
        }

        stage('Test') {
            steps {
                echo 'Checking that result.txt exists'
                sh 'test -f result.txt'

                echo 'Checking the file content'
                sh "grep -q 'Hello from GitHub and Jenkins!' result.txt"
            }
        }
    }

    post {
        always {
            archiveArtifacts(
                artifacts: 'result.txt',
                fingerprint: true
            )
        }

        success {
            echo 'All checks passed'
        }

        failure {
            echo 'One or more checks failed'
        }
    }
}

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
        sh '''
            expected='Hello from GitHub and Jenkins!'
            actual=$(cat result.txt)

            echo "Expected: $expected"
            echo "Actual:   $actual"

            if [ "$actual" != "$expected" ]; then
                echo "ERROR: result.txt contains unexpected text"
                exit 1
            fi

            echo "Content check passed"
        '''
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

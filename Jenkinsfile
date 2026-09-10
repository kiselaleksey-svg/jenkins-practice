pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'stage', 'prod'],
            description: 'Choose environment'
        )

        choice(
            name: 'BROWSER',
            choices: ['chrome', 'firefox'],
            description: 'Choose browser'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Repository: ${env.GIT_URL}"
                echo "Branch: ${env.GIT_BRANCH}"
            }
        }

        stage('Show parameters') {
            steps {
                echo "Environment: ${params.ENVIRONMENT}"
                echo "Browser: ${params.BROWSER}"
            }
        }

        stage('Create file') {
            steps {
                writeFile(
                    file: 'result.txt',
                    text: """Environment=${params.ENVIRONMENT}
Browser=${params.BROWSER}
"""
                )
            }
        }

        stage('Run tests') {
            steps {
                echo 'Checking that result.txt exists'
                sh 'test -f result.txt'

                echo 'Checking the file content'
                sh """
                    set -eu

                    echo 'Expected environment: ${params.ENVIRONMENT}'
                    echo 'Expected browser: ${params.BROWSER}'

                    echo 'Actual file content:'
                    cat result.txt

                    grep -Fxq 'Environment=${params.ENVIRONMENT}' result.txt || {
                        echo 'ERROR: Environment does not match'
                        exit 1
                    }

                    grep -Fxq 'Browser=${params.BROWSER}' result.txt || {
                        echo 'ERROR: Browser does not match'
                        exit 1
                    }

                    echo 'All content checks passed'
                """
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
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'One or more checks failed'
        }
    }
}
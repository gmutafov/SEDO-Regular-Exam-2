pipeline {
    agent any

    stages {
        stage('Detect branch') {
            steps {
                script {
                    def branch = env.BRANCH_NAME ?: env.GIT_BRANCH ?: ''

                    echo "Detected branch: '${branch}'"

                    branch = branch.replaceFirst(/^origin\//, '')

                    if (branch != 'main') {
                        echo "Not main branch. Aborting pipeline."
                        currentBuild.result = 'SUCCESS'
                        error("Aborting build: branch is not main")
                    }
                }
            }
        }

        stage('Restore') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test --no-build --verbosity normal'
            }
        }
    }
}

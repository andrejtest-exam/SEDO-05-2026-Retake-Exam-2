pipeline {
    agent any

    // Trigger a build when changes are pushed to GitHub.
    // The Jenkins job's SCM is configured to track the 'main' branch,
    // so this pipeline builds and tests whenever main is updated.
    triggers {
        githubPush()
    }

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = '1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore Homies.sln'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build Homies.sln --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test Homies.sln --configuration Release --no-build --logger "trx;LogFileName=test-results.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/*.trx', allowEmptyArchive: true
        }
    }
}

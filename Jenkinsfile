pipeline {
    agent any

    stages {
        stage('Checkout code') {
            // Checkout the repository
            steps {
                git branch 'main', url: 'https://github.com/viktoriyadi/JenkinsSeleniumIde_7_30'
            }
        }

        stage('Set up .NET CORE') {
            // Install dotnet
            steps {
                bat '''
                echo Downloading .Net 6 Sdk
                curl -l -0 dotnet-sdk-6.0.424-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/23c7bf0d-e22d-4372-bcb2-292eb36a5238/11af494be409759f46b679ab22e65a58/dotnet-sdk-6.0.424-win-x64.exe
                echo installing dotnet-sdk-6.0.424-win-x64.exe
                dotnet-sdk-6.0.424-win-x64.exe /quiet /norestart
                '''
            }
        }

        stage('Restore dependencies') {
            // install dependencies
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }

        stage('Build') {
            // build
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }

        stage('Run Tests') {
            // build
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}

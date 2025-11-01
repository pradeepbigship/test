
pipeline {
    agent any

    environment {
        DOTNET_PATH = "C:\\Program Files\\dotnet"
        PATH = "${env.DOTNET_PATH};${env.PATH}"
        WEBAPP_PATH = "C:\\inetpub\\newfolder\\"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/pradeepbigship/test.git'
            }
        }

        stage('Restore Dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build Application') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Publish to Folder') {
            steps {
                bat 'dotnet publish -c Release -o publish_output'
            }
        }

       stage('Deploy to IIS') {
    steps {
        // Restart a specific application pool (e.g., "MyAppPool")
        bat '''
        powershell -Command "Stop-WebAppPool -Name 'uploadimageurl'; Start-WebAppPool -Name 'uploadimageurl"
        '''
        
        // Copy files to IIS folder
        bat 'xcopy publish_output "%WEBAPP_PATH%" /E /H /C /Y'

        // Optionally restart IIS if necessary
        // bat 'iisreset'
            }
        }
    }
}

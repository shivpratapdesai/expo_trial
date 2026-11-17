pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting source code from GitHub'
                checkout scm
            }
        }

        stage('Verify Files') {
            steps {
                echo 'Checking application files'

                bat '''
                    if not exist index.html exit /b 1
                    if not exist style.css exit /b 1
                    if not exist script.js exit /b 1
                    echo All application files found!
                '''
            }
        }

        stage('Validate Application') {
            steps {
                echo 'Validating HTML file'

                bat '''
                    findstr /I /C:"<html>" index.html >nul
                    if errorlevel 1 exit /b 1

                    findstr /I /C:"</html>" index.html >nul
                    if errorlevel 1 exit /b 1

                    echo HTML validation successful!
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Application build completed'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts:
                    'index.html,style.css,script.js',
                    fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Jenkins pipeline completed!'
        }

        failure {
            echo 'FAILURE: Please check the console output.'
        }
    }
}
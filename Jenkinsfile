pipeline {
    agent any

    tools {
        nodejs 'Node.js 20 LTS'
    }

    options {
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                "C:\\Windows\\System32\\cmd.exe" /c npm install
                '''
            }
        }

        stage('Build React App') {
            steps {
                bat '''
                "C:\\Windows\\System32\\cmd.exe" /c npm run build
                '''
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                if not exist "C:\\ProgramData\\Jenkins\\.jenkins\\userContent\\attendance-management-system" (
                    mkdir "C:\\ProgramData\\Jenkins\\.jenkins\\userContent\\attendance-management-system"
                )

                C:\\Windows\\System32\\xcopy.exe "build\\*" "C:\\ProgramData\\Jenkins\\.jenkins\\userContent\\attendance-management-system\\" /E /I /Y
                '''
            }
        }

        stage('Archive Build') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful!'
        }

        failure {
            echo 'Build Failed!'
        }

        always {
            cleanWs()
        }
    }
}
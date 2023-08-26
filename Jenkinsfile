pipeline {
    agent any
    
    environment {
        NODE_VERSION = "14"
        SERVER_CREDENTIALS = credentials('nexus-credentials')
    }

    tools {
        nodejs "NodeJS ${NODE_VERSION}"
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Compress Artifacts') {
            steps {
               dir('dist/') {
                zip zipFile: '../my-artifacts.zip', archive: false
            }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                echo "Deploying with ${SERVER_CREDENTIALS_USR}"
            }
        }
    }
}

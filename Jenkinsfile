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
                script {
                    def uniqueIdentifier = env.BUILD_NUMBER // Use BUILD_NUMBER or another unique identifier
                    
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'dist/**', onlyIfSuccessful: true, fingerprint: true
                    archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
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

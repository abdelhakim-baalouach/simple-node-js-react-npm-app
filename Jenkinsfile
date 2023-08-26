pipeline {
    agent {
        label 'your-build-agent-label' // Use a specific build agent or 'any' for any available agent
    }
    
    environment {
        SERVER_CREDENTIALS = credentials('nexus-credentials')
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10')) // Keep a limited number of build logs
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Compress Artifacts') {
            steps {
                sh 'zip -r myapp.zip dist/*'
            }
        }

        stage('deploy') {
            steps {
                echo 'deploying the application...'
                //echo "deplying with ${SERVER_CREDENTIALS}"
                sh('echo ${SERVER_CREDENTIALS_USR} ${SERVER_CREDENTIALS_PSW}')
                
            }
        }
    }
}

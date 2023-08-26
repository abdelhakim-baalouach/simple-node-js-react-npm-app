pipeline {
    agent any
    
    environment {
        SERVER_CREDENTIALS = credentials('nexus-credentials')
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

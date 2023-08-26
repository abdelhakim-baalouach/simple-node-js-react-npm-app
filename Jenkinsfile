pipeline {
    agent any
    
    environment {
        SERVER_CREDENTIALS = credentials('nexus-credentials')
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    def nodeTool = tool name: "NodeJS 14", type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${nodeTool}/bin:${env.PATH}"
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                def nodeTool = tool name: "NodeJS 14", type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                env.PATH = "${nodeTool}/bin:${env.PATH}"
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

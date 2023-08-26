pipeline {
    agent any
    
    environment {
        NODE_VERSION = "14"
        SERVER_CREDENTIALS = credentials('nexus-credentials')
        NEXUS_REPO_URL = 'http://localhost:9000/repository/demo_CI_CD/'
        NEXUS_USERNAME = "${SERVER_CREDENTIALS_USR}"
        NEXUS_PASSWORD = "${SERVER_CREDENTIALS_PWD}"
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
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'dist/**'
                }
            }
        }

         stage('Deploy to Nexus') {
            steps {
                script {
                    def zipFilePath = "archive.zip" // Replace with the actual path to your zip file
                    def nexusApiUrl = "${NEXUS_REPO_URL}/${zipFilePath}"

                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        sh "curl -v -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file ${zipFilePath} ${nexusApiUrl}"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
    
    environment {
        NODE_VERSION = "14"
        NEXUS_REPO_URL = 'https://localhost:9000/repository/demo_CI_CD/'
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
                    def zipFilePath = "${JENKINS_HOME}/workspace/${JOB_NAME}/archive.zip" // Adjust the path to the zip file
                    def nexusApiUrl = "${NEXUS_REPO_URL}archive.zip"

                     sh "curl -v -u admin:Sys*6SFL** --upload-file ${zipFilePath} ${nexusApiUrl}"
                }
            }
        }
    }
}

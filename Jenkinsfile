pipeline {
    agent any

    environment {
        NODE_VERSION = "14"
        NEXUS_REPO_URL = 'http://localhost:9000/repository/demo_ci_cd/'
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
                    def zipFileName = "artifacts.zip"
                    def zipFilePath = "${JENKINS_HOME}/workspace/demo_master/${zipFileName}"

                    // Compress the artifacts into a ZIP file
                    sh "zip -r ${zipFilePath} build/*"

                    // Archive the ZIP file as an artifact
                    archiveArtifacts artifacts: "${zipFilePath}", allowEmptyArchive: true

                    // Set a variable for the ZIP file URL
                    def zipFileUrl = "${env.BUILD_URL}artifact/${zipFileName}"

                    echo "ZIP file URL: ${zipFileUrl}"
                }
            }
        }

    }
}
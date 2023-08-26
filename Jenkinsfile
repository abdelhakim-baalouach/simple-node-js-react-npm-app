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
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'build/**'
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                node {
                    stage('Check ZIP File Existence') {
                        // Define the path to the ZIP file
                        def zipFilePath = '/var/jenkins_home/workspace/demo/master/archive.zip'
                        
                        // Check if the ZIP file exists
                        def zipFileExists = fileExists(zipFilePath)
                        
                        if (zipFileExists) {
                            echo "ZIP file exists"
                            
                            // Perform actions with the ZIP file (e.g., upload using curl)
                            withCredentials([string(credentialsId: 'NEXUS_PASSWORD', variable: 'NEXUS_PASSWORD')]) {
                                sh """
                                    curl -v -u admin:\$NEXUS_PASSWORD --upload-file ${zipFilePath} http://localhost:9000/repository/demo_ci_cd/
                                """
                            }
                        } else {
                            error "ZIP file does not exist"
                        }
                    }
                }



                /*script {
                    def workspacePath = "${JENKINS_HOME}/workspace/demo/master"
                    def zipFilePath = "${workspacePath}/archive.zip"
        
                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        //sh "curl -v -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file ${zipFilePath} ${NEXUS_REPO_URL}"
                        sh "curl -v -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file ${zipFilePath} ${NEXUS_REPO_URL}/"
                    }
                }*/
            }
        }
    }
        
}
// Function to check if a file exists
def fileExists(filePath) {
    def file = new File(filePath)
    return file.exists()
}

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
                script {
                    def zipFilePath = "${JENKINS_HOME}/workspace/demo/master/archive.zip"
                    if (fileExists(zipFilePath)) {
                        echo "ZIP file exists"
                        currentBuild.result = 'SUCCESS'
                    } else {
                        error "ZIP file does not exist"
                    }
                }
            }
        }
        
        stage('Upload ZIP File') {
            when {
                expression {
                    return currentBuild.resultIsBetterOrEqualTo('SUCCESS')
                }
            }
            steps {
                script {
                    def workspacePath = "${JENKINS_HOME}/workspace/demo/master"
                    def zipFilePath = "${workspacePath}/archive.zip"
        
                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        sh "curl -v -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file ${zipFilePath} ${NEXUS_REPO_URL}/"
                    }
                }
            }
        }
    }
}

// Function to check if a file exists using the fileExists step
def fileExists(filePath) {
    return step([$class: 'FileExists', file: filePath])
}

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
            stage('Check ZIP File Existence') {
            steps {
                script {
                    def zipFilePath = '/var/jenkins_home/workspace/demo/master/archive.zip'
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
                withCredentials([string(credentialsId: 'NEXUS_PASSWORD', variable: 'NEXUS_PASSWORD')]) {
                    script {
                        def zipFilePath = '/var/jenkins_home/workspace/demo/master/archive.zip'
                        sh """
                            curl -v -u admin:\$NEXUS_PASSWORD --upload-file ${zipFilePath} http://localhost:9000/repository/demo_ci_cd/
                        """
                    }
                }
            }
        }
    }
        
}
// Function to check if a file exists
def fileExists(filePath) {
    def file = new File(filePath)
    return file.exists()
}

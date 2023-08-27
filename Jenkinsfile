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

        stage('Check File Existence') {
            steps {
                script {
                    def filePath = "${JENKINS_HOME}/workspace/demo/master/archive.zip"
                    def fileExists = sh(script: "if [ -f ${filePath} ]; then echo 'true'; else echo 'false'; fi", returnStatus: true).trim() == 'true'

                    if (fileExists) {
                        echo "File exists"
                    } else {
                        echo "File does not exist"
                    }
                }
            }
        }

        stage('Deploy to Nexus repository') {
            when {
                expression { return currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
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

        stage('check if ZIP file exists') {
            steps {
                script {
                    def filePath = "${JENKINS_HOME}/workspace/demo/master/archive.zip"
                    def file = new File(filePath)
                    if (file.exists()) {
                        echo "ZIP file exists"
                        currentBuild.result = 'SUCCESS'
                    } else {
                        error "ZIP file does not exist"
                    }
                }
            }
        }

        stage('Deploy to Nexus repository') {
            when {
                expression { return currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
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



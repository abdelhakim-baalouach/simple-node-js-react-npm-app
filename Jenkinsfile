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

        stage('Deploy to Nexus repository') {
            steps {
                script {
                    def workspacePath = "${JENKINS_HOME}/workspace/demo_master/"
                    def zipFilePath = "${workspacePath}/build/"

                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        sh "curl -v -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file ${zipFilePath} ${NEXUS_REPO_URL}/"
                    }
                }
            }
        }
    }
}
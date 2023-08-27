pipeline {
    agent any

    environment {
        NEXUS_REPO_URL = 'http://localhost:9000/repository/demo_ci_cd/'
    }

    stages {
        stage('Deploy to Nexus repository') {
            steps {
                script {
                    def workspacePath = "${JENKINS_HOME}/workspace/demo_master/"
                    def zipFilePath = "${workspacePath}/build.zip"

                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        sh "curl -v -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file ${zipFilePath} ${NEXUS_REPO_URL}/"
                    }
                }
            }
        }
    }
}
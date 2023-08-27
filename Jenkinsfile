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
    }
}
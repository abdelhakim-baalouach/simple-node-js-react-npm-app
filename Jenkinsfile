@NonCPS
def checkFileExists(filePath) {
    def file = new File(filePath)
    return file.exists()
}

pipeline {
    agent any

    stages {
        stage('Check File Existence') {
            steps {
                script {
                    def filePath = 'path/to/your/file.txt'
                    def fileExists = checkFileExists(filePath)

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


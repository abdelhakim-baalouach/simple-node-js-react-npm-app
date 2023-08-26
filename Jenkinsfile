pipeline {
    agent any
    environment {
        NEW_VERSION = '1.3.0'
        SERVER_CREDENTIALS = credentials('nexus-credentials')
    }
    //tools {
        
    //}
    
    stages {
        stage('Build') {
            steps {
                echo 'building the application...'
                echo "building version ${NEW_VERSION}"
            }
        }

        stage('test') {
            steps {
                echo 'testing the application...'
            }
        }

        stage('deploy') {
            steps {
                echo 'deplying the application...'
                //echo "deplying with ${SERVER_CREDENTIALS}"
                //sh "${SERVER_CREDENTIALS}"
                withCredentials([
                    usernamePassword(credentials: 'nexus-credentials', usernameVariable: USER, passwordVariable: PWD)
                ]) {
                    sh "some script ${USER} ${PWD}"
                }
            }
        }
    }

    post {
        always {
            echo 'always...'
        }
        success {
            echo 'success...'
        }
        failure {
            echo 'failure...'
        }
    }
}

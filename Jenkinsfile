pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                dcoker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Build steps'
                sh '''
                    ls -al
                    node --version
                    npm --verion
                    npm ci
                    npm run build
                    ls -al
                    node --version
                    npm --version
                '''
            }
        }
    }
}
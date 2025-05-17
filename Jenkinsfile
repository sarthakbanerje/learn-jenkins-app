pipeline {
    agent any

    envionment {
        NETLIFY_PROJECT_ID = '1595e748-0c17-473c-bbbf-f3e3d846350d'
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Build steps'
                sh '''
                    ls -al
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -al
                    node --version
                    npm --version
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Test steps'
                sh '''
                    test -f '/var/jenkins_home/workspace/learn-jerkins-app/build/index.html'
                    npm test
                '''
            }
        }
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Deploy steps'
                sh '''
                    npm install netlify-cli@20.1.1
                    ./node_modules/.bin/netlify --version
                    echo 'Deploying to Production. Project ID: $NETLIFY_PROJECT_ID'
                '''
            }
        }
    }
}
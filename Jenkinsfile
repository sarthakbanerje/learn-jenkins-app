pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '1595e748-0c17-473c-bbbf-f3e3d846350d'
        NETLIFY_AUTH_TOKEN = credentials('netlify')
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

        stage('Deploy Stage') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Deploy steps'
                sh '''
                    echo 'Deploy Step for Staging'
                    npm install netlify-cli@20.1.1 node-jq
                    ./node_modules/.bin/netlify --version                    
                    echo "Deploying to Production. Project ID: $NETLIFY_PROJECT_ID"
                    ./node_modules/.bin/netlify deploy --dir=./build --json > deploy-out.json
                    ./node_modules/.bin/node-jq -r '.deploy_url' deploy-out.json
                '''
            }
        }
        stage('Approval') {
            steps {
                echo 'Manual Approval Required'
                timeout(5) {
                    input message: '', ok: 'Yes, I am sure'
                }
            }
        }
        stage('Deploy Prod') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Deploy steps'
                sh '''
                    echo 'Deploy Step for Production'
                    npm install netlify-cli@20.1.1
                    ./node_modules/.bin/netlify --version                    
                    echo "Deploying to Production. Project ID: $NETLIFY_PROJECT_ID"
                    ./node_modules/.bin/netlify deploy --dir=./build --prod
                '''
            }
        }
    }
}
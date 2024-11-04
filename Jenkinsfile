pipeline {
    agent any

    stages {        
        stage('Build - with docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh ''' 
                    ls -la
                    node --version
                    npm --version
                    echo "Building app with docker"
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test - with docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh ''' 
                    test -f build/index.html
                    npm test
                '''
            }
        }
        
        stage('Deploy - with docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh ''' 
                    ls -la
                    node_modules/.bin/npm install netlify-cli
                    netlify --version
                    ls -la
                '''
            }
        }
        
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

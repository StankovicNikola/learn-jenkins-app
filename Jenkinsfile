pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '4becb97d-7c9d-464c-adf5-63af2c625d5b'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "deploying to prod. site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
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

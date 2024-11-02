pipeline {
    agent any

    stages {
        // stage('Without docker') {
        //     steps {
        //         sh '''
        //             echo "Without docker"
        //             ls -la
        //             touch container-no.txt
        //         '''
                
        //     }
        // }
        
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
    }
}

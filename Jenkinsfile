pipeline {
    agent any

    stages {
        stage('Without docker') {
            steps {
                sh '''
                    echo "Without docker"
                    ls -la
                    touch container-no.txt
                '''
                
            }
        }
        
        stage('With docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh ''' 
                    echo "With docker"
                    npm --version
                    ls -la
                    touch container-yes.txt
                '''
            }
        }
    }
}

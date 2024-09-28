pipeline {
    agent any

    environment {
        FILE_NAME = 'index.html'
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
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps{
                sh ''' 
                    echo 'STARTING TEST STAGE'
                    test -f build/$FILE_NAME
                    npm test
                '''
            }
        }
    }
}
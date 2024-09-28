pipeline {
    agent any

    environment {
        FILE_NAME = 'index.html'
    }

    stages {
        /*
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
        */
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh ''' 
                    echo 'STARTING TEST STAGE'
                    test -f build/${FILE_NAME}
                    npm test
                '''
            }
        }
        stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh ''' 
                    echo 'STARTING E2E STAGE'
                    npm install -g serve
                    serve -s build
                    npx playwright test
                '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
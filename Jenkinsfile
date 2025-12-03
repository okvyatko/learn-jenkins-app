pipeline {
    agent any
    
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
                    npm -version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Unit Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Unit Test Stage'
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('E2E Test') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.57.0-noble'
                    reuseNode true
                }
            }
            steps {
                echo 'Initializing server'
                sh '''
                    npm install -g serve
                    serve -s build
                '''
                echo 'Server Initialized'
            }
        }
    }

    post {
        always {
            sh 'test -f test-results/junit.xml'
            junit 'test-results/junit.xml'
        }
    }
}

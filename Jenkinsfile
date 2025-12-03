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
                    # Start server in background

                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                    
                    # Stop server
                    pkill -f "serve -s build" || true
                '''
                echo 'Server Initialized'
            }
        }
    }

    post {
        always {
            sh 'test -f jest-test-results/junit.xml'
            junit 'jest-test-results/junit.xml'
        }
    }
}

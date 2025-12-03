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
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                echo 'Initializing server'
                    sh '''
                        npm install serve
                        nohup node_modules/.bin/serve -s build > serve.log 2>&1 &
                        echo "Server started"
                        sleep 5

                        npx playwright test
                    '''
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

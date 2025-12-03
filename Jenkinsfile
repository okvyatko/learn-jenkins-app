pipeline {
    agent{
        docker{
            image 'node:18-alpine'
            reuseNode true
        }
    }
    stages {
        stage ('build'){
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
            steps {
                echo 'Unit Test Stage'
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }
        stage('E2E Test Stage') {
            agent {
                docker {
                    image 'node:18-alpine'
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

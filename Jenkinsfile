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
        stage('Test') {
            steps {
                echo 'Test Stage'
                sh '''
                test -f build/index.html'
                npm test
                '''
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

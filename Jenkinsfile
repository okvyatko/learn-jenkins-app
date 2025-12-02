pipeline {
    agent any

    stages {
        stage ('build'){
            agent{
                docker{
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
        stage('Test') {
            steps {
                echo 'Test Stage'
                script{
                    if (fileExists('build/index.html')) {
                        echo "file: build/index.html found!"
                    }
                    else{
                        throw new Exception("File not found: build/index.html")
                    }
                }
                sh 'npm test'
            }
        }
    }
}

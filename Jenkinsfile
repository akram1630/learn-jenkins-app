pipeline {
    agent any

    stages {
        stage('Build') {
          // Docker agent removed because Jenkins container has Node
            // agent{ 
            //     docker{
            //         image 'node:18-alpine'
            //         reuseNode true
            //     } 
            // }
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
        stage('Test') {
            // agent {
            //     docker {
            //         image 'node:18-alpine'
            //         reuseNode true
            //     }
            // }

            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('Deploy') {
            // Docker agent removed because Jenkins container has Node
            // agent {
            //     docker {
            //         image 'node:18-alpine'
            //         reuseNode true
            //     }
            // }
            steps {
                sh '''
                    npm install netlify-cli
                    netlify --version
                '''
            }
        }
    }
    
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

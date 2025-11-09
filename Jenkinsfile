pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'e77c23c2-8db7-4ad2-a790-1ce450294ca1'
    }
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
                    echo "deploying to production. Site ID: $NETLIFY_SITE_ID"
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

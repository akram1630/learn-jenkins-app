pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'PUT HERE YOUR SITE ID!'
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                    echo "Listing project files"
                    ls -la
                    echo "Node version:"
                    node --version
                    echo "NPM version:"
                    npm --version
                    echo "Installing dependencies"
                    npm ci
                    echo "Building project"
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Tests') {
            parallel {

                stage('Unit tests') {
                    steps {
                        sh '''
                            # Create test-results folder
                            mkdir -p test-results
                            echo "Running unit tests"
                            npm test
                        '''
                    }
                    post {
                        always {
                            junit 'test-results/junit.xml'
                        }
                    }
                }

                stage('E2E') {
                    steps {
                        sh '''
                            echo "Installing Playwright and serve"
                            npm install --save-dev playwright serve
                            echo "Installing Playwright browsers"
                            npx playwright install
                            echo "Starting static server"
                            node_modules/.bin/serve -s build &
                            sleep 10
                            echo "Running Playwright E2E tests"
                            npx playwright test --reporter=html
                        '''
                    }

                    post {
                        always {
                            publishHTML([
                                allowMissing: false, 
                                alwaysLinkToLastBuild: false, 
                                keepAll: false, 
                                reportDir: 'playwright-report', 
                                reportFiles: 'index.html', 
                                reportName: 'Playwright HTML Report', 
                                useWrapperFileDirectly: true
                            ])
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Installing Netlify CLI"
                    npm install netlify-cli@20.1.1
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                '''
            }
        }
    }
}

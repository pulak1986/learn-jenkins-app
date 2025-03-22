pipeline {
    agent any

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
                script {
                    sh '''
                    echo "Running npm ci..."
                    npm ci
                    echo "Building the project..."
                    npm run build
                    '''
                }
            }
        }
        */

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    // Install dependencies if not installed yet
                    sh 'npm ci'
                    
                    // Check if build/index.html exists
                    sh 'test -f build/index.html'
                    
                    // Run unit tests
                    sh 'npm test'
                }
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                script {
                    // Install dependencies and serve the build
                    sh 'npm ci'
                    sh 'npm install serve'

                    // Serve the build output
                    sh 'node_modules/.bin/serve -s build &'

                    // Wait for the server to be ready
                    sh '''
                    for i in {1..30}; do
                        if curl -s http://localhost:5000 > /dev/null; then
                            echo "Server is ready."
                            break
                        fi
                        echo "Waiting for server to be ready..."
                        sleep 1
                    done
                    '''

                    // Run E2E tests with Playwright
                    sh 'npx playwright test'
                }
            }
        }
    }

    post {
        always {
            // Archive test results (make sure your test results are in the right directory)
            junit 'jest-results/junit.xml'
        }
    }
}
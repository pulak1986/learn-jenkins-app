pipeline {
    agent any

    stages {
        /*
        stage('Build'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    // List files and check node/npm versions
                    sh '''
                    echo "Listing files before npm install:"
                    ls -la
                    echo "Node version:"
                    node --version
                    echo "NPM version:"
                    npm --version
                    
                    echo "Running npm ci..."
                    npm ci
                    
                    echo "Listing files after npm install:"
                    ls -la

                    echo "Running npm build..."
                    npm run build
                    '''
                }
            }
        } */
         stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
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
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }
        post {
            
        always {
            junit 'jest-results/junit.xml'
        }
    }
    }
    }


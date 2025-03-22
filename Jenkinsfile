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
    }
}

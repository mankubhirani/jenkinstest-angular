pipeline {
    agent any

    environment {
        NODE_VERSION = '18.20.6'  // Ensure Node.js version is set correctly
    }

    stages {
        stage('Setup Node.js') {
            steps {
                script {
                    def nodeCheck = sh(script: 'node -v', returnStatus: true)
                    if (nodeCheck != 0) {
                        error "Node.js is not installed. Install it before running this pipeline."
                    }
                }
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/mankubhirani/jenkinstest-angular.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    def npmCheck = sh(script: 'npm -v', returnStatus: true)
                    if (npmCheck != 0) {
                        error "NPM is not installed. Please install Node.js with NPM."
                    }
                    
                    def npmCache = sh(script: 'npm cache verify', returnStatus: true)
                    if (npmCache != 0) {
                        sh 'npm cache clean --force'
                    }

                    def nodeModulesExists = sh(script: 'test -d node_modules && echo "Exists" || echo "Missing"', returnStdout: true).trim()
                    if (nodeModulesExists == "Missing") {
                        sh 'npm install'
                    } else {
                        echo "Node modules already installed. Skipping npm install."
                    }
                }
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build --configuration=production'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'npm test'
            }
        }
    }

    post {
        success {
            echo '✅ Build and Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed! Check logs for more details.'
        }
    }
}

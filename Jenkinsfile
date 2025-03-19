pipeline {
    agent any

    environment {
        NODE_VERSION = '18.20.6'  // Ensure Node.js version is set correctly
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/mankubhirani/jenkinstest-angular.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    def npmCache = sh(script: 'npm cache verify', returnStatus: true)
                    if (npmCache != 0) {
                        sh 'npm cache clean --force'
                    }
                    sh 'npm install'
                }
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build --prod'
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
            echo 'Build and Deployment Successful!'
        }
        failure {
            echo 'Build Failed! Check logs for more details.'
        }
    }
}

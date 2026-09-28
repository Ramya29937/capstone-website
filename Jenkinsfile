pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub'
                checkout scm
            }
        }

        stage('Verify Application') {
            steps {
                echo 'Verifying application files'
                sh 'ls -la'
                sh 'test -f Dockerfile'
                sh 'test -f index.html'
            }
        }

        stage('Production Release') {
            when {
                expression {
                    return new Date().format('dd') == '25'
                }
            }
            steps {
                echo 'Today is the 25th. Production release is allowed.'
                echo 'Deploying application to Kubernetes production cluster'
            }
        }
    }

    post {
        success {
            echo 'Jenkins Pipeline completed successfully'
        }
        failure {
            echo 'Jenkins Pipeline failed'
        }
    }
}

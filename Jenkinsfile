pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Validate Application') {
            steps {
                sh '''
                    test -f Dockerfile
                    test -f index.html
                    echo "Dockerfile and index.html verified successfully"
                '''
            }
        }

        stage('Production Release') {
            when {
                expression {
                    return new Date().format('dd') == '25'
                }
            }

            steps {
                echo '25th of the month - deploying application to Kubernetes'

                sh '''
                    ssh -o StrictHostKeyChecking=no \
                    -i /var/lib/jenkins/jenkins-key.pem \
                    ubuntu@13.210.242.249 \
                    "kubectl set image deployment/website-deployment website=ramya2901/website-app:latest && \
                    kubectl scale deployment/website-deployment --replicas=2 && \
                    kubectl rollout status deployment/website-deployment --timeout=120s"
                '''
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

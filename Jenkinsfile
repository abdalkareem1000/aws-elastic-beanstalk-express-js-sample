pipeline {
    agent {
        // Use Node.js 16 Docker image as the build agent
        docker {
            image 'node:16'
            args '-v /var/run/docker.sock:/var/run/docker.sock'  // Mount Docker if needed for further steps
        }
    }

    environment {
        // Fetch Snyk API token from Jenkins credentials (make sure this is stored securely in Jenkins credentials)
        SNYK_TOKEN = credentials('snyk-api-token')  
    }

    stages {
        stage('Install Dependencies') {
            steps {
                // Install Node.js project dependencies using npm
                sh 'npm install --save'
            }
        }

        stage('Build') {
            steps {
                // You can add other build-related steps here if needed
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests, if available, using npm
                sh 'npm test'
            }
        }

        // New stage for Snyk vulnerability scanning
        stage('Snyk Security Scan') {
            steps {
                echo 'Running Snyk security scan...'
                // Install Snyk globally within the pipeline environment
                sh 'npm install -g snyk'
                // Authenticate Snyk using the token stored in Jenkins credentials
                sh 'snyk auth $SNYK_TOKEN'
                // Run Snyk test with a high severity threshold to halt on critical/high vulnerabilities
                sh 'snyk test --severity-threshold=high'
            }
        }
    }

    post {
        success {
            echo 'Build, tests, and security scan completed successfully!'
        }
        failure {
            echo 'Build, tests, or security scan failed.'
        }
    }
}

pipeline {
    agent {
        // Use Node.js 16 Docker image as the build agent
        docker {
            image 'node:16'
            args '-v /var/run/docker.sock:/var/run/docker.sock'  // Mount Docker if needed for further steps
        }
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
    }
    post {
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}

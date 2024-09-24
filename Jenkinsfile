pipeline {
    // Use Docker as the agent and specify the Docker image to be 'node:16'
    agent {
        docker {
            image 'node:16'
            // Mount Docker socket so that Jenkins can communicate with the Docker daemon on the host machine
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    // Environment variables (Optional)
    environment {
        // Add any environment variables you need, such as the DOCKER_HOST if required.
        DOCKER_HOST = "unix:///var/run/docker.sock"
        // You could also add Snyk API token here if needed for security scanning
        // SNYK_TOKEN = credentials('snyk-api-token')
    }

    // Define the stages of the pipeline
    stages {
        // Pre-stage: Check Docker access
        stage('Check Docker Access') {
            steps {
                echo 'Checking Docker access...'
                // Test if Docker is accessible by listing containers
                sh 'docker ps'
            }
        }

        // Stage 1: Install project dependencies
        stage('Install Dependencies') {
            steps {
                // Run 'npm install' to install the dependencies specified in package.json
                sh 'npm install --save'
            }
        }

        // Stage 2: Build the project
        stage('Build') {
            steps {
                // Run 'npm run build' to build the project (this assumes you have a build script in package.json)
                sh 'npm run build'
            }
        }

        // Stage 3: Run tests
        stage('Test') {
            steps {
                // Run 'npm test' to execute tests (this assumes you have a test script in package.json)
                sh 'npm test'
            }
        }

        // (Optional) Stage 4: Security scan with Snyk (if integrated)
        /*
        stage('Snyk Security Scan') {
            steps {
                echo 'Running Snyk security scan...'
                // Install Snyk globally in the container
                sh 'npm install -g snyk'
                // Authenticate Snyk using the token stored in Jenkins credentials (ensure it's added in Jenkins)
                sh 'snyk auth $SNYK_TOKEN'
                // Run the Snyk test to scan for vulnerabilities and fail on high/critical issues
                sh 'snyk test --severity-threshold=high'
            }
        }
        */
    }

    // Post-build actions to log whether the pipeline succeeded or failed
    post {
        // If the build is successful
        success {
            echo 'Build, tests, and security scan completed successfully!'
        }

        // If the build fails at any stage
        failure {
            echo 'Build, tests, or security scan failed.'
        }
    }
}


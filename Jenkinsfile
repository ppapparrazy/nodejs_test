pipeline {
    agent any // Run on any available agent

    environment {
        NODE_ENV = 'production' // Set environment variables
        NODE_VERSION = '18'     // Specify Node.js version
    }

    stages {
        // Stage 1: Checkout the code from the repository
        stage('Checkout') {
            steps {
                checkout scm // Checkout the code from the SCM (e.g., Git)
            }
        }

        // Stage 2: Set up Node.js environment
        stage('Setup Node.js') {
            steps {
                script {
                    // Install the specified Node.js version using nvm
                    sh '''
                        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
                        export NVM_DIR="$HOME/.nvm"
                        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
                        nvm install ${NODE_VERSION}
                        nvm use ${NODE_VERSION}
                    '''
                }
            }
        }

        // Stage 3: Install dependencies
        stage('Install Dependencies') {
            steps {
                sh 'npm install' // Use 'yarn install' if you use Yarn
            }
        }

        // Stage 4: Run tests
        stage('Run Tests') {
            steps {
                sh 'npm test' // Use 'yarn test' if you use Yarn
            }
            post {
                success {
                    echo 'Tests passed successfully!'
                }
                failure {
                    echo 'Tests failed!'
                    error('Pipeline failed due to test failures') // Fail the pipeline
                }
            }
        }

        // Stage 5: Build the project
        stage('Build') {
            steps {
                sh 'npm run build' // Use 'yarn build' if you use Yarn
            }
        }

        // Stage 6: Deploy (Optional)
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Add deployment steps here (e.g., deploy to AWS, Heroku, etc.)
            }
        }
    }

    post {
        // Actions to perform after the pipeline completes
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs() // Clean up the workspace
        }
    }
}
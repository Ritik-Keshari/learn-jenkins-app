pipeline {
    agent any

    options {
        timeout(time: 10, unit: 'MINUTES')  // Set timeout to avoid long-running jobs
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                script {
                    try {
                        echo "Starting the build process"

                        // Display current directory contents before starting
                        sh 'echo "Initial Directory Listing:"'
                        sh 'ls -la'

                        // Show versions of node and npm to ensure environment is set up correctly
                        sh '''
                            echo "Node version: $(node --version)"
                            echo "NPM version: $(npm --version)"
                        '''

                        // Clean install dependencies and check for errors
                        echo "Running npm ci..."
                        sh 'sudo npm ci --verbose'

                        // Show directory contents after npm install
                        sh 'echo "Directory Listing After npm ci:"'
                        sh 'ls -la'

                        // Build the project and capture output
                        echo "Running npm run build..."
                        sh 'npm run build --verbose'

                        // Show directory contents after build
                        sh 'echo "Directory Listing After Build:"'
                        sh 'ls -la'

                        echo "Build process completed successfully."
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error "Build failed: ${e.getMessage()}"
                    }
                }
            }
        }
    }
}

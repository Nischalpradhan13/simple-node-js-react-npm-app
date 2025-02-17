pipeline {
    agent { label "Alma" }  // This specifies your Alma Linux agent
    
    environment {
        CI = 'true'
    }
    
    stages {
        stage('Setup') {
            steps {
                // Ensure Docker is available on agent
                sh 'docker --version'
                
                // Pull the Node image
                sh 'docker pull node:6-alpine'
            }
        }
        
        stage('Build') {
            steps {
                // Run npm install inside Docker container
                sh '''
                    docker run --rm \
                    -v "${WORKSPACE}:/app" \
                    -w "/app" \
                    node:6-alpine \
                    npm install
                '''
            }
        }
        
        stage('Test') {
            steps {
                // Run tests inside Docker container with CI mode
                sh '''
                    docker run --rm \
                    -v "${WORKSPACE}:/app" \
                    -w "/app" \
                    -e CI=true \
                    node:6-alpine \
                    npm test -- --watchAll=false
                '''
            }
        }
        
        stage('Deliver') {
            steps {
                // Make deliver script executable
                sh 'chmod +x ./jenkins/scripts/deliver.sh'
                
                // Run deliver script in Docker
                sh '''
                    docker run --rm \
                    -v "${WORKSPACE}:/app" \
                    -w "/app" \
                    -p 3000:3000 \
                    -d \
                    --name node-app \
                    node:6-alpine \
                    ./jenkins/scripts/deliver.sh
                '''
                
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                
                // Kill the container
                sh 'docker stop node-app || true'
                sh 'docker rm node-app || true'
            }
        }
    }
}

pipeline {
    agent any 
    environment { 
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') 
    } 
    stages { 
        stage('Build') { 
            steps { 
                sh 'echo "Building the application..."' 
                sh 'docker build -t my-python-app .' 
            } 
        } 
        stage('Test') { 
            steps { 
                sh 'echo "Running tests..."' 
                // Add your unit test commands here
            } 
        } 
        stage('Push to Docker Hub') { 
            steps { 
                sh 'echo "Pushing the Docker image to Docker Hub..."' 
                // Secure login using --password-stdin
                sh """
                    echo \${DOCKER_HUB_CREDENTIALS_PSW} | \
                    docker login -u \${DOCKER_HUB_CREDENTIALS_USR} --password-stdin
                """
                // Fixed line breaks in tag/push commands
                sh "docker tag my-python-app \${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest" 
                sh "docker push \${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest" 
            } 
        } 
        stage('Deploy') { 
            steps { 
                sh 'echo "Deploying the application..."' 
                // Consolidated SSH commands for better readability
                sh """
                    ssh user@remote-server '
                        docker pull \${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest && \
                        docker stop my-python-app || true && \
                        docker rm my-python-app || true && \
                        docker run -d -p 5000:5000 --name my-python-app \${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest
                    '
                """
            } 
        }
    }
    post {
        always {
            sh 'docker logout'  // Always logout after build
        }
    }
}
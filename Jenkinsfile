pipeline { 
    agent any 

    environment { 
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') 
    } 

    stages { 
        stage('Build') { 
            steps { 
                sh 'echo "Building the application..."' 
                sh 'docker build -t atelier4_jenkins .' 
            } 
        } 
        
        stage('Test') { 
            steps { 
                sh 'echo "Running tests..."' 
                // Ajoutez ici des commandes pour exécuter des tests unitaires 
            } 
        } 
        
        stage('Push to Docker Hub') { 
            steps { 
                sh 'echo "Pushing the Docker image to Docker Hub..."' 
                sh "docker login -u ${DOCKER_HUB_CREDENTIALS_USR} -p ${DOCKER_HUB_CREDENTIALS_PSW}"
                sh "docker tag atelier4_jenkins ${DOCKER_HUB_CREDENTIALS_USR}/atelier4_jenkins:latest"
                sh "docker push ${DOCKER_HUB_CREDENTIALS_USR}/atelier4_jenkins:latest" 
            } 
        } 
        
        stage('Deploy') { 
            steps { 
                sh 'echo "Deploying the application..."' 
                sh "ssh user@remote-server 'docker pull ${DOCKER_HUB_CREDENTIALS_USR}/atelier4_jenkins:latest'" 
                sh "ssh user@remote-server 'docker stop atelier4_jenkins || true'" 
                sh "ssh user@remote-server 'docker rm atelier4_jenkins || true'" 
                sh "ssh user@remote-server 'docker run -d -p 5000:5000 --name atelier4_jenkins ${DOCKER_HUB_CREDENTIALS_USR}/atelier4_jenkins:latest'" 
            } 
        } 
    }
}
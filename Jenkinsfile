pipeline {
    agent {
        docker {
            image 'docker:latest'
        }
    }
    
    environment {
        DOCKER_REGISTRY = 'ranjan' // Your Docker Hub username
        DOCKER_IMAGE = 'myapp'    // The name of your Docker image
    }

    stages {
    
        // Stage 1: Checkout the repository code
        stage('Checkout Code') {
            steps {
                git 'https://github.com/ranjansigdel/be-safe-ci-cd-pipeline.git'
            }
        }
        
        // Stage 2: Run automated tests
        stage('Build & Test') {
            steps {
                // Replace this command with your actual test logic
                sh './run-tests.sh'
            }
        }
        
        // Stage 3: Build and Push Docker image to Docker Hub
        stage('Docker Build & Push') {
            steps {
                // Log in to Docker Hub
                sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                
                // Build Docker image
                sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:latest .'
                
                // Push Docker image to the registry
                sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
            }
        }
        
        // Stage 4: Deploy the Docker image
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8080:8080 $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
            }
        }
    }
}

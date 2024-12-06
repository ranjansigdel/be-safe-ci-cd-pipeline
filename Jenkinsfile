pipeline {
    agent {
        label 'your-node-label' // Replace with your Jenkins agent node if configured, otherwise use 'any'
    }

    environment {
        DOCKER_REGISTRY = 'ranjan'
        DOCKER_IMAGE = 'myapp'
    }

    stages {
    
        stage('Checkout Code') {
            steps {
                git 'https://github.com/ranjansigdel/be-safe-ci-cd-pipeline.git'
            }
        }

        stage('Run Tests') {
            steps {
                sh './run-tests.sh'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:latest .'
                sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8080:8080 $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
            }
        }
    }
}

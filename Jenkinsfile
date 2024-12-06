node { 
    environment {
        DOCKER_REGISTRY = 'ranjan'
        DOCKER_IMAGE = 'myapp'
    }

    stage('Checkout Code') {
        git 'https://github.com/ranjansigdel/be-safe-ci-cd-pipeline.git'
    }

    stage('Build & Test') {
        sh './run-tests.sh'
    }

    stage('Docker Build & Push') {
        sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
        sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:latest .'
        sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
    }

    stage('Deploy') {
        sh 'docker run -d -p 8080:8080 $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
    }
}

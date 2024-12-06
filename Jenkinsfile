pipeline {
    agent any // Use any available Jenkins agent for execution

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
                sh './scripts/test.sh'
            }
        }

        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:latest .'
                    sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8080:8080 $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
            }
        }
    }

    post {
        always {
            sh 'docker system prune -f' // Cleanup unused images/containers
        }
    }
}

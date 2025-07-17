pipeline {
    agent any

    environment {
        IMAGE_NAME = "your-dockerhub-username/flask-gists-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install --upgrade pip'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '. venv/bin/activate && pytest test_app.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Deploy (Optional)') {
            steps {
                echo 'You can define your Kubernetes / Docker Swarm / VM deployment here if required.'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up Docker cache...'
            sh 'docker system prune -f || true'
        }
    }
}

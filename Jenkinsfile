pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_REPO', defaultValue: 'flask-gists-app', description: 'DockerHub Repository Name')
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker Image Tag')
    }

    environment {
        IMAGE_NAME = "gopi1806/${params.IMAGE_REPO}"
        IMAGE_TAG = "${params.IMAGE_TAG}"
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
                withCredentials([usernamePassword(credentialsId: 'aa3bf5ae-39bd-49b8-8eb0-6045f97ae27f', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
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

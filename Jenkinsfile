pipeline {
    agent {
        docker {
            image 'python:3.11-slim'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'python -m unittest test_app.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-flask-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop AWS-Jenkins-pipeline || true
                docker rm AWS-Jenkins-pipeline || true
                docker run -d -p 80:80 --name AWS-Jenkins-pipeline my-flask-app
                '''
            }
        }
    }
}

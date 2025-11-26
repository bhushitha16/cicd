pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub_creds')
        IMAGE_NAME = "bhushitha16/calculator-app"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/bhushitha16/cicd'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin"
                sh "docker push ${IMAGE_NAME}:latest"
            }
        }
    }

    post {
        success {
            echo "CI/CD completed — Docker image pushed to DockerHub!"
        }
        failure {
            echo "Build failed — check console logs."
        }
    }
}


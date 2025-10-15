pipeline {
    agent any

    tools {
        maven 'Maven 3'
    }

    environment {
        DOCKER_IMAGE = "shivay2525/dockerapp:latest"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Swadhera25/DO-ise3.git'
            }
        }

        stage('Build Java Application') {
            steps {
                bat 'mvn -B clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat """
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    docker push %DOCKER_IMAGE%
                    docker logout
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker image built and pushed successfully!"
        }
        failure {
            echo "❌ Build or push failed!"
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKER_REPO = 'GokuWNL'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sourabh00465/Anime-Cards.git'
            }
        }

        stage('Build Backend') {
            steps {
                dir('hokage') {
                    sh 'mvn clean package -DskipTests'
                    sh "docker build -t $DOCKER_REPO/anime-backend:latest ."
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('anime-frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                    sh "docker build -t $DOCKER_REPO/anime-frontend:latest ."
                }
            }
        }

        stage('Push Images') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    sh "docker push $DOCKER_REPO/anime-backend:latest"
                    sh "docker push $DOCKER_REPO/anime-frontend:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
}

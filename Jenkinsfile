pipeline {
    agent any

    environment {
        IMAGE_NAME = 'trien_khai_pham_mem'
        VERSION = '1.0'
    }

    stages {
        stage('Clone Source Code') {
            steps {
                echo 'Cloning repository...'
                // Jenkins tự động clone khi cấu hình Git trong Job
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image for ASP.NET Core...'
                sh '''
                    docker build -t ${IMAGE_NAME}:${VERSION} .
                '''
            }
        }

        stage('Clean Old Containers') {
            steps {
                echo 'Stopping & removing old containers...'
                sh '''
                    docker compose -f docker-compose-server.yaml down || true
                    docker compose -f docker-compose-node.yaml down || true
                '''
            }
        }

        stage('Deploy Backend (ASP.NET Core)') {
            steps {
                echo 'Starting backend container...'
                sh '''
                    docker compose -f docker-compose-server.yaml up -d --build
                '''
            }
        }

        stage('Deploy Frontend (NodeJS Client nếu có)') {
            steps {
                echo 'Starting frontend container...'
                sh '''
                    docker compose -f docker-compose-node.yaml up -d --build
                '''
            }
        }

        stage('Done') {
            steps {
                echo 'Deployment to local server completed!'
            }
        }
    }
}

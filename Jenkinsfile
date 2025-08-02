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
                // Jenkins tự động clone nếu đã khai báo Git repo trong Job
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image for ASP.NET Core...'
                bat '''
                    docker build -t %IMAGE_NAME%:%VERSION% .
                '''
            }
        }

        stage('Clean Old Containers') {
            steps {
                echo 'Stopping & removing old containers...'
                bat '''
                    docker compose -f docker-compose-server.yaml down || exit 0
                    docker compose -f docker-compose-node.yaml down || exit 0
                '''
            }
        }

        stage('Deploy Backend (ASP.NET Core)') {
            steps {
                echo 'Starting backend container...'
                bat '''
                    docker compose -f docker-compose-server.yaml up -d --build
                '''
            }
        }

        stage('Deploy Frontend (NodeJS Client nếu có)') {
            steps {
                echo 'Starting frontend container...'
                bat '''
                    docker compose -f docker-compose-node.yaml up -d --build
                '''
            }
        }

        stage('Done') {
            steps {
                echo '🎉 Local deployment completed on Windows!'
            }
        }
    }
}

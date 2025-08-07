pipeline {
    agent any

    environment {
        IMAGE_NAME = 'trien_khai_pham_mem'
        VERSION = '1.0'
        PROJECT_DIR = 'Trien_khai_pham_mem' // Thay tên thư mục nếu cần
    }

    stages {
        stage('Clone Source Code') {
            steps {
                echo 'Cloning repository...'
                // Jenkins sẽ tự động clone nếu Git repo đã được khai báo
            }
        }

        stage('Build Docker Image') {
    steps {
        echo 'Building Docker image for ASP.NET Core...'
        bat '''
            docker build -t trien_khai_pham_mem:1.0 .
        '''
            }
        }

        stage('Clean Old Containers') {
            steps {
                echo 'Stopping & removing old containers...'
                bat """
                    cd %PROJECT_DIR%
                    docker compose -f docker-compose-server.yaml down || exit 0
                    docker compose -f docker-compose-node.yaml down || exit 0
                """
            }
        }

        stage('Deploy Backend (ASP.NET Core)') {
            steps {
                echo 'Starting backend container...'
                bat """
                    cd %PROJECT_DIR%
                    dir
                    if exist .grafana.secret (
                        type .grafana.secret
                    ) else (
                        echo File .grafana.secret not found!
                        exit /b 1
                    )
                    docker compose -f docker-compose-server.yaml up -d --build
                """
            }
        }

        stage('Deploy Frontend (NodeJS Client nếu có)') {
            steps {
                echo 'Starting frontend container...'
                bat """
                    cd %PROJECT_DIR%
                    docker compose -f docker-compose-node.yaml up -d --build
                """
            }
        }

        stage('Deploy Monitoring Stack (Grafana, Prometheus, etc)') {
            steps {
                echo 'Starting monitoring stack...'
                bat """
                    cd %PROJECT_DIR%
                    docker compose -f docker-compose-node.yaml -f docker-compose-server.yaml up -d --build
                """
            }
        }

        stage('Done') {
            steps {
                echo 'Local deployment completed on Windows!'
            }
        }
    }
}

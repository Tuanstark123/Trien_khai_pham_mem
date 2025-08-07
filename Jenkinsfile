pipeline {
    agent any

    environment {
        REMOTE_USER = 'it23'
        REMOTE_HOST = '101.99.23.156'
        REMOTE_PORT = '22001'
        REMOTE_PASS = credentials('ssh-password')  // Tạo credentials ID là 'ssh-password'
        PROJECT_DIR = 'Trien_khai_pham_mem'
        SSH_OPTIONS = '-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
    }

    stages {
        stage('Deploy to Remote Server') {
            steps {
                echo 'Starting deployment to remote server...'
                bat """
                    sshpass -p %REMOTE_PASS% ssh %SSH_OPTIONS% -p %REMOTE_PORT% %REMOTE_USER%@%REMOTE_HOST% ^
                    "cd %PROJECT_DIR% || git clone https://github.com/Tuanstark123/Trien_khai_pham_mem.git && cd %PROJECT_DIR% && git pull"
                """
            }
        }

        stage('Clean Old Containers (Remote)') {
            steps {
                echo 'Stopping and removing old containers on server...'
                bat """
                    sshpass -p %REMOTE_PASS% ssh %SSH_OPTIONS% -p %REMOTE_PORT% %REMOTE_USER%@%REMOTE_HOST% ^
                    "cd %PROJECT_DIR% && docker compose -f docker-compose-server.yaml down || true && docker compose -f docker-compose-node.yaml down || true"
                """
            }
        }

        stage('Run Backend (ASP.NET Core) on Server') {
            steps {
                echo 'Running backend container on server...'
                bat """
                    sshpass -p %REMOTE_PASS% ssh %SSH_OPTIONS% -p %REMOTE_PORT% %REMOTE_USER%@%REMOTE_HOST% ^
                    "cd %PROJECT_DIR% && docker compose -f docker-compose-server.yaml up -d --build"
                """
            }
        }

        stage('Run Frontend (NodeJS) on Server') {
            steps {
                echo 'Running frontend container on server...'
                bat """
                    sshpass -p %REMOTE_PASS% ssh %SSH_OPTIONS% -p %REMOTE_PORT% %REMOTE_USER%@%REMOTE_HOST% ^
                    "cd %PROJECT_DIR% && docker compose -f docker-compose-node.yaml up -d --build"
                """
            }
        }

        stage('Run Monitoring Stack (Grafana, Prometheus, etc) on Server') {
            steps {
                echo 'Running monitoring stack on server...'
                bat """
                    sshpass -p %REMOTE_PASS% ssh %SSH_OPTIONS% -p %REMOTE_PORT% %REMOTE_USER%@%REMOTE_HOST% ^
                    "cd %PROJECT_DIR% && docker compose -f docker-compose-server.yaml -f docker-compose-node.yaml up -d --build"
                """
            }
        }

        stage('Done') {
            steps {
                echo 'Remote deployment completed successfully!'
            }
        }
    }
}

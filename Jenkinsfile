pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ecommerce-app'
        CONTAINER_NAME = 'ecommerce-container'
        PUBLISH_DIR = 'publish_output'
    }

    stages {
        stage('Clean') {
            steps {
                bat "if exist %PUBLISH_DIR% rmdir /s /q %PUBLISH_DIR%"
            }
        }

        stage('Restore') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                bat 'dotnet test'
            }
        }

        stage('Publish') {
            steps {
                bat "dotnet publish -c Release -o %PUBLISH_DIR%"
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Docker Run') {
            steps {
                // Nếu container đang chạy, dừng và xóa nó
                bat """
                docker stop %CONTAINER_NAME% || echo No running container
                docker rm %CONTAINER_NAME% || echo No container to remove
                docker run -d -p 8180:8080 --name %CONTAINER_NAME% %IMAGE_NAME% 
                """
            }
        }

        // Optional: Push lên Docker Hub nếu muốn
        // stage('Docker Push') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
        //             bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
        //             bat "docker tag %IMAGE_NAME% yourdockerhub/%IMAGE_NAME%"
        //             bat "docker push yourdockerhub/%IMAGE_NAME%"
        //         }
        //     }
        // }
    }
}

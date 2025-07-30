pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ecommerce-app'
    }

    stages {
        stage('Clean') {
            steps {
                bat 'if exist publish_output rmdir /s /q publish_output'
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
                bat 'dotnet publish -c Release -o publish_output'
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %IMAGE_NAME% ."
            }
        }

        // Optional: Push image to Docker Hub
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

pipeline {
    agent any

    tools {
        dotnet 'dotnet7' // hoặc dotnet6 nếu bạn dùng .NET 6, tên này phải khớp với Jenkins config
    }

    stages {
        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'dotnet test'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o publish_output'
            }
        }
    }
}

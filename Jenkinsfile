pipeline {
    agent any
    environment {
        dotnet = 'C:\\Program Files\\dotnet\\dotnet.exe'
    }
    stages {
        stage('Checkout Stage') {
            steps {
                git credentialsId: '5ba5e0da-116a-47df-8e8c-639f4654358c', url: 'https://github.com/Jaydeep-007/JenkinsWebApplicationDemo.git', branch: 'main'
            }
        }
        stage('Build Stage') {
            steps {
                bat '"%dotnet%" build "%WORKSPACE%\\JenkinsWebApplicationDemo.sln" --configuration Release'
            }
        }
        stage('Test Stage') {
            steps {
                bat '"%dotnet%" test "%WORKSPACE%\\TestProject1\\TestProject1.csproj"'
            }
        }
        stage("Release Stage") {
            steps {
                bat '"%dotnet%" build "%WORKSPACE%\\JenkinsWebApplicationDemo.sln" /p:PublishProfile="%WORKSPACE%\\JenkinsWebApplicationDemo\\Properties\\PublishProfiles\\FolderProfile.pubxml" /p:Platform="Any CPU" /p:DeployOnBuild=true /m'
            }
        }
        stage('Deploy Stage') {
            steps {
                // Dừng IIS trước khi deploy
                bat 'net stop "w3svc"'
                // Thực hiện deploy với Microsoft Web Deploy
                bat '"C:\\Program Files (x86)\\IIS\\Microsoft Web Deploy V3\\msdeploy.exe" -verb:sync -source:package="%WORKSPACE%\\JenkinsWebApplicationDemo\\bin\\Debug\\net6.0\\JenkinsWebApplicationDemo.zip" -dest:auto -setParam:"IIS Web Application Name"="Demo.Web" -skip:objectName=filePath,absolutePath=".\\\\PackagDemoeTmp\\\\Web.config$" -enableRule:DoNotDelete -allowUntrusted=true'
                // Khởi động lại IIS
                bat 'net start "w3svc"'
            }
        }
    }
}

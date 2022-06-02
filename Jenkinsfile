pipeline {
    agent any
     triggers {
        githubPush()
      }
    stages {
        stage('Restore packages'){
           steps{
               bat 'dotnet restore JenkinsDotNetCoreCICDpipelineApp.sln'
            }
         }        
        stage('Clean'){
           steps{
               bat 'dotnet clean JenkinsDotNetCoreCICDpipelineApp.sln --configuration Release'
            }
         }
        stage('Build'){
           steps{
               bat 'MSBuild /t:restore JenkinsDotNetCoreCICDpipelineApp.sln --configuration Release --no-restore'
            }
         }
        stage('Test: Unit Test'){
           steps {
                bat 'dotnet test XUnitTestProject/XUnitTestProject.csproj --configuration Release --no-restore'
             }
          }
        stage('Publish'){
             steps{
               bat 'dotnet publish WebApplication/JenkinsDotNetCoreCICDpipeline.csproj --configuration Release --no-restore'
             }
        }
        stage('Deploy'){
             steps{
               //bat '''for pid in $(lsof -t -i:9090); do
                //       kill -9 $pid
              // done'''
               bat 'cd WebApplication/bin/Release/netcoreapp3.1/publish'
               bat 'dotnet publish JenkinsDotNetCoreCICDpipeline.dll --urls="http://20.232.129.22:9090" --ip="20.232.129.229" --port=9090 --no-restore > /dev/null 2>&1 &'
             }
        }        
    }
}

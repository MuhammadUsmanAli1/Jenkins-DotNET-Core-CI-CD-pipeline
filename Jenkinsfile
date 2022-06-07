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
               bat 'dotnet build JenkinsDotNetCoreCICDpipelineApp.sln --configuration Release --no-restore'
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
        
        stage('make zip') {
            steps {
                echo "working"
               script {
                    zip dir: 'WebApplication/bin/Release/netcoreapp3.1/publish', glob: '', zipFile: 'test.zip'
                }
            }
        }
         stage('Copy to s3') {
        s3Upload(file:'test.zip', bucket:'SmsHandler', path:'')
       }
          stage('Create Application') {
            steps {
                  echo "working"
                //bat  "echo Create Application"
                //bat 'aws elasticbeanstalk create-application-version --application-name php  --version-label jenkins-pipeline-26 --source-bundle S3Bucket=jenkins-backup-files-sa,S3Key=myzip.zip'
            }
        }
          stage('Update to Beanstalk') {
            steps {
                  echo "working"
                //bat  "echo Update to Beanstalk"
                //bat 'aws elasticbeanstalk update-environment --application-name php --environment-name Php-env --version-label jenkins-pipeline-26'
            }
        }        
        
    }}

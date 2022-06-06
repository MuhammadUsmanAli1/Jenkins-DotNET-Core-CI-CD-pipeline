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
               //bat 'zip -r myzip.zip *'
            }
        }
         stage('Copy to s3') {
            steps {
                 
                bat  "echo Copy to S3"
                bat '''
                        echo "Buils starting..."'
                        CD C:\ProgramData\Jenkins\.jenkins\workspace\ivatech2\WebApplication\bin\Release\netcoreapp3.1
                        cd
                    '''                
                //bat 'dir C:\ProgramData\Jenkins\.jenkins\workspace\ivatech2\WebApplication\bin\Release\netcoreapp3.1'
                
                bat 'tar -a -c -f compressed.zip * '
                bat 'aws s3 cp myzip.zip s3://jenkins-backup-files-sa'
            }
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

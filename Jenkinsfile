pipeline {
    agent any

     options {
        //Disable concurrentbuilds for the same job
        disableConcurrentBuilds()
        // Colorize the console log
        ansiColor("xterm")          
        // Add timestamps to console log
        timestamps()
        
    }

    environment {
        AWS_ACCESS_KEY = credentials('aws-access-key')
        AWS_SECRET_KEY = credentials('aws_secret_key')
    }

    stages {

        stage('Build Lambda') {
            steps {
                echo 'Build'
        
        
                sh 'mvn clean install'             
            }
        }
        stage('Test') {
            steps {
                echo 'Test'
                // sh 'mvn test'
            }
        }
        stage('Push to artifactory') {
            steps {
                echo 'Push to artifactory'
            }
        }

        stage('Deploy to QA') {
            steps {
                echo 'Deploy to QA'
                sh 'zip -g bermtec-0.0.1.zip **/target'
                 
                 sh 'aws --version'
                 sh 'aws s3 ls'
                 sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY'
                 sh 'aws configure set aws_secret_access_key $AWS_SECRET_KEY'
                //  sh 'aws configure set region us-east-1' 
                sh 'aws s3 cp bermtec-0.0.1.zip s3://bermtec28/lambdatest'
                 echo "Stage 2 Yes"
                 sh 'aws lambda update-function-code --function-name test  --zip-file fileb://bermtec-0.0.1.zip'              
            }
        }

        stage('Release to Prod') {
            steps {
                echo 'Release to Prod'
                script {
                    if (env.BRANCH_NAME == "master") {
                        input('Proceed for Prod Deployment ?')
                    }
                }

            }
        }

         stage('Deploy to Prod') {
            steps {
                echo 'Deploy to Prod'
                sh 'aws s3 cp bermtec-0.0.1.zip s3://bermtec28/lambdaprod'
                sh 'aws lambda update-function-code --function-name prodfunction  --zip-file fileb://bermtec-0.0.1.zip'  

            }
        }

    }
    post {
      failure {
        echo 'failed'
             mail to: 'teambermtec@gmail.com',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_NUMBER}"
      }
      success {
        echo 'Success'
      }
      aborted {
        echo 'aborted'
      }
    }
}
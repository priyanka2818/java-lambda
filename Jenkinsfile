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
        AWS_ACCESS_KEY = 'AKIAWABC6GF6UR3DXLHZ'
        AWS_SECRET_KEY = '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'
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
                sh 'zip -g bermtec-0.0.1.zip .'
                 
                 sh 'aws --version'
                 sh 'aws configure set aws_access_key_id $AWS_ACCESS_KEY'
                 sh 'aws configure set aws_secret_access_key $AWS_SECRET_KEY'
                 sh 'aws configure set region us-east-1' 
                 echo "Stage 2 Yes"
                 sh 'aws lambda update-function-code --function-name test  --zip-file fileb://bermtec-0.0.1.zip'              
            }
        }

        stage('Deploy to Prod') {
            steps {
                echo 'Deploy to Prod'
            }
        }
    }
    post {
      failure {
        echo 'Failed'
      }
      success {
        echo 'Success'
      }
      aborted {
        echo 'aborted'
      }
    }
}
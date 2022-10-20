pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn clean package'             
            }
        }
        stage('Test') {
            steps {
                echo 'Test'
                // sh 'mvn test'
            }
        }
        stage('Sonar Analysis') {
            steps {
                echo 'SOnar Qube'
                sh "mvn sonar:sonar -Dsonar.projectKey=Raghupatik_java-springboot"
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
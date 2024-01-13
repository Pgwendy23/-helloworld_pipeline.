pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '358729504175.dkr.ecr.us-east-1.amazonaws.com/devops_repository'
    region = 'us-east-1'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Pgwendy23/-helloworld_pipeline..git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('docker login'){
            steps{
                script{
                    sh 'aws ecr get-login-password --region "${region}"| docker login --username AWS --password-stdin "${registry}"'
                }
            }
        }

        stage('Deploy image') {
            steps{
                script{ 
                    dockerImage.push()
                }
            }
        }  
    }
}

pipeline {
    agent any
    stages {
        stage('Checkout'){
            steps {
                checkout scm
            }  
        }
        stage('Build') {
            steps {
//                 sh 'cd /var/lib/jenkins/workspace/Deploy-Sample-Java-Application/'
                sh 'mvn install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Dockerise') {
            steps {
                sh 'docker build . -t chinpan111999/demo-java-app:${BUILD_NUMBER}'
            }
        }
        stage('Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'Docker-hub-Jenkins', passwordVariable: 'PASSWD', usernameVariable: 'USERNAME')]) {
                    sh 'docker login -u "$USERNAME" -p "$PASSWD"'
                    sh 'docker push chinpan111999/demo-java-app:${BUILD_NUMBER}'
                    }
            }
        }
        stage('Deploy to Test'){
            steps{
                sh 'docker run --name Test-Env-${BUILD_NUMBER} chinpan111999/demo-java-app:${BUILD_NUMBER}'
            }
        }
        stage('Deploy approval'){
          steps{
              input "Deploy to PROD?"
            }
        }

        stage('Deploy to Prod'){
            steps{
                sh 'docker run --name Prod-Env-${BUILD_NUMBER} chinpan111999/demo-java-app:${BUILD_NUMBER}'
            }
        }
    }
}

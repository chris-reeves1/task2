pipeline {
    agent any

    stages {
         stage('Init') {
            steps {
                sh 'docker rm -f $(docker ps -qa)'
            }
        }
        stage('Build') {
            steps {
                sh 'chmod +x deploy.sh'
                sh './deploy.sh'
            }
        }

        stage('push') {
            steps {
                sh 'ls' 
            }
        }
    }
}
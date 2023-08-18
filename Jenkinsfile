pipeline {
    agent {
	    label "agent"
    }
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }

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

        stage('Push') {      	
            steps{                       	
	            sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"                		
	            echo 'Login Completed'
                sh "docker tag trio-task-mysql:5.7 chrisreeves1/mytriotasksql:latest"
                sh "docker tag trio-task-flask-app chrisreeves1/mytriotaskflaskapp:latest"
                sh "docker push chrisreeves1/mytriotasksql:latest"
                sh "docker push chrisreeves1/mytriotaskflaskapp:latest"
            }           
        }
    }
}

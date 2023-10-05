pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Init') {
            steps {
                script {
                    sh 'docker rm -f $(docker ps -qa) || true'
                    sh 'docker network create trio-task-network || true'
                    sh 'docker volume create new-volume'
                }
            }
        }
        stage('Build and Run Containers') {
            steps {
                script {
                    // Build the custom images
                    def dbImage = docker.build("trio-task-mysql:5.7", "db")
                    def appImage = docker.build("trio-task-flask-app:latest", "flask-app")
                    
                    // Run containers
                    dbImage.run("-d --name mysql --network trio-task-network -v new-volume:/var/lib/mysql")
                    appImage.run("-d -e MYSQL_ROOT_PASSWORD=password --name flask-app --network trio-task-network")
                    
                    // Use standard Nginx image and bind mount for custom configuration
                    docker.image('nginx:latest').run("-d --name nginx -p 80:80 --network trio-task-network -v \$(pwd)/nginx/nginx.conf:/etc/nginx/nginx.conf")
                }
            }
        }
        stage('Push Images to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        def appImage = docker.image("trio-task-flask-app:latest")
                        def dbImage = docker.image("trio-task-mysql:5.7")
                        
                        appImage.push("chrisreeves1/mytriotaskflaskapp-dev-work")
                        dbImage.push("chrisreeves1/mytriotasksql-dev-work")
                    }
                }
            }
        }
    }
}

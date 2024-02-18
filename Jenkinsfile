pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/Apoorvag242/django-notes-app.git", branch: "main"
            } 
        }
        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t my-note-app1 ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")]) {
                    sh "docker tag my-note-app1 ${env.dockerhubuser}/my-note-app1:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/my-note-app1:latest"    
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                // Add your deployment steps here
                sh "docker-compose down && docker-compose uo -d "
            }
        }
    }
}

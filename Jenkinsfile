pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'Cloning the code'
                git url:"https://github.com/prameymayekar/django-notes-app.git", branch: "main"
            }
        }
         stage('Build') {
            steps {
                echo 'Building an image'
                sh "docker build -t my-note-app ."
            }
        }
         stage('Push to docker') {
    steps {
        echo 'Pushing image to DockerHub'
        withCredentials([usernamePassword(credentialsId: "docker-cred", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubUser")]) {
            script {
                // Tag the image with DockerHub repo and latest tag
                sh "docker tag my-note-app ${dockerHubUser}/my-note-app:latest"
                
                // Log in to DockerHub using the credentials
                sh "echo ${dockerHubpass} | docker login -u ${dockerHubUser} --password-stdin"
                
                // Push the image to DockerHub
                sh "docker push ${dockerHubUser}/my-note-app:latest"
            }
        }
    }
}
         stage('Deploy') {
            steps {
                echo 'App deploy'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage("Code"){
            steps {
                echo "Clone Code"
                git url:"https://github.com/suniltechnology/django-node-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps {
                echo "Build the image"
                sh "docker build -t node-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag node-app ${env.dockerHubUser}/node-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

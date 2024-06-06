pipeline {
    agent any
    stages {
        stage("code"){
            steps {
            echo "cloning the code" 
            git url:"https://github.com/Sunilmargale/django-notes-app.git", branch: "master"
            }
        }
        stage("build"){
            steps {
                echo "building the image"
                sh "docker build -t notes-app ."
            }
        }
        stage("push to dockerhub"){
            steps {
                echo "pushing the image to dockerhub"
                withCredentials([usernamePassword(credentialsId:"docker-hub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"  
                sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            } 
        }
        stage("deploy"){
            steps {
                echo "deploying the container"
                sh "docker run -d -p 8000:8000 sunilmargale/notes-app:latest"
            }
        }
    }
}

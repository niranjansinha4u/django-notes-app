pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "cloning the GitHub repo code.."
                git url: "https://github.com/niranjansinha4u/django-notes-app.git", branch:"main"
            }
            
        }
        
        stage("Build Code") {
            steps {
                echo "Building new image.."
                sh "docker build -t my-notes-app-demo ."
            }
            
        }
        
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-notes-app-demo ${env.dockerHubUser}/my-notes-app-demo:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-notes-app-demo:latest"
                }
            }
        }
        
        stage("Deploy Code") {
            steps {
                echo "Deploying the container.."
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}

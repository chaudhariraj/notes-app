
pipeline {
    agent any
    
    stages{
        stage("Clone github Code"){
            steps {
                echo "Cloning the code"  
                git url: "https://github.com/chaudhariraj/notes-app.git", branch: "dev"
            }
            
        }
        stage("Build"){
            steps { 
                echo "Building the code"
                sh "docker build -t my-note ."
                
            }
            
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]){
                sh "docker tag my-note ${env.dockerHubUser}/my-note:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}" 
                sh "docker push ${env.dockerHubUser}/my-note:latest"
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

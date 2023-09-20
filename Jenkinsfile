pipeline    {
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "Cloning the code"
                git url:"https://github.com/ulabbasi/django-notes-app", branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image"
                sh "docker build -t umairabbasid/notes-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag notes-app ${env.dockerhubuser}/notes-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push umairabbasid/notes-app"
                }
                
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
        
    }
}

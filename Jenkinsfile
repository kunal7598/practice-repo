pipeline {
    agent { label "vinod" }
    
    stages {
        stage("copying code"){
            steps{
                echo "This is cloning the code"
                git url: "https://github.com/kunal7598/practice-repo.git", branch: "main"
                echo "code-cloning successfull"
            }
        }
        
        stage("building code"){
            steps{
                echo "Building the code"
                sh "docker build -t notes-app:latest ."
            }
        }
        
        stage("pushing to dockerhub"){
            steps{
            withCredentials([usernamePassword(credentialsId: 'dockerHubCred', passwordVariable: 'dockerHubPass', usernameVariable: 
            'dockerHubUser')]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
            }
        }
    }
        
        stage("deploying app"){
            steps{
                echo "Deploying the code"
                sh "docker compose up -d"
            }
        }
    }
}

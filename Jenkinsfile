pipeline {
    agent { label "dev-agent" }
    stages{
        stage("Clone Code"){
            steps{
                script{ 
                    properties([pipelineTriggers([pollSCM('')])])
                }
                git url: "https://github.com/PrabirKumarMahatha/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build . -t prabirmahatha/node-todo-app-cicd:latest"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push prabirmahatha/node-todo-app-cicd:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

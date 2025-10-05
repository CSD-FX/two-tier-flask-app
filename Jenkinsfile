pipeline {
    agent any;
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/CSD-FX/two-tier-flask-app.git" , branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build --no-cache -t 2-tier-app ."
            }
        }
        stage("Test"){
            steps{
                echo "skip"
            }
        }
        stage("Docker-Hub-Push_Pull"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "DOCKER-HUB-CRED",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag 2-tier-app ${env.dockerHubUser}/2-tier-app"
                sh "docker push ${dockerHubUser}/2-tier-app"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
            }
        }
        
    }
}

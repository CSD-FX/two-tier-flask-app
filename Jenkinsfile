pipeline {
    agent { label "dev" };
    stages{
        stage("Code"){
            steps{
                git url:"https://github.com/CSD-FX/two-tier-flask-app.git" , branch: "master"
            }
        }
        
        stage("Build"){
            steps{
                sh "docker build -t flaskappv1 ."
            }
        }
        
        stage("Test"){
            steps{
                echo "Testing is done"
            }
        }
        
        stage("Push"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "DockerCred",
                    passwordVariable: "dockerpass",
                    usernameVariable: "dockeruser"
                    )]){
                    
                sh "docker login -u ${env.dockeruser} -p ${env.dockerpass}"
                sh "docker image tag flaskappv1 ${env.dockeruser}/flaskappv1"
                sh "docker push ${env.dockeruser}/flaskappv1:latest"
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

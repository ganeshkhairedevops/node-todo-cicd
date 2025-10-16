pipeline{
    agent any;
    stages{
        stage("code clone"){
            steps{
                echo "Code Clone Stage"
                git url:"https://github.com/ganeshkhairedevops/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Trivy File System Scan"){
            steps{
                echo "trivy scan"
                sh "trivy fs ."
            }
        }
        stage("build"){
            steps{
                echo "code build"
                sh "docker build -t node-app ."
            }
        }
        stage("docker push"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                 )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app ${env.dockerHubUser}/node-app"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build"
            }
        }
    }
}

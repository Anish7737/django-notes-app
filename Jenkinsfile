pipeline {
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/Anish7737/django-notes-app.git", branch: "main"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build -t django-notes-app ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"DockerHub",
                        passwordVariable:"DockerHubPass", 
                        usernameVariable:"DockerHubUser"
                        )
                    ]
                ){
                sh "docker tag django-notes-app ${env.DockerHubUser}/django-notes-app:latest"
                sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                sh "docker push ${env.DockerHubUser}/django-notes-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose up && docker-compose down -d" 
            }
        }
    }
}

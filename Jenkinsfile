pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                git url: "https://github.com/dheeraj1kumar/loan.git", branch: "main"
            }
        }
        

        stage('Build') {
            steps {
                sh '''
                      docker build -t django-app:latest .
                '''
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhub",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                sh "docker image tag branch-app:latest ${env.dockerHubUser}/branch-app:latest"
                sh "docker push ${env.dockerHubUser}/branch-app:latest"
                }
            }
        }


        stage('Test') {
            steps {
                echo "Test the code"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy the app"
            }
        }
    }
}
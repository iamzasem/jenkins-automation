pipeline {
    agent any

    stages {
        stage ('Fetch Code') {
            steps{
                echo 'Cloning Code From Github'
                git url:'https://github.com/codeboylal/Project-8-Django-new-todo.git', branch: 'main'
            }
        }
        
        stage ('Building Code') {
            steps{
                echo 'Building Code to Docker'
                sh 'docker build -t static-web-dashboard .'       
            }
        }

        stage ('Push to DockerHub') {
            steps {
                echo 'Pushing image to dockerhub'
                withCredentials([usernamePassword(credentialsId: 'static-web-connection', usernameVariable: 'dockerhubUser', passwordVariable: 'dockerhubPass')]) {
                    sh "docker tag static-web-dashboard $dockerhubUser/static-web-dashboard:latest"
                    sh "docker login -u $dockerhubUser -p $dockerhubPass"
                    sh "docker push $dockerhubUser/static-web-dashboard:latest"
                }
            }
        }

        stage ('Deploy') {
            steps {
                echo 'Deploying to docker container'
                sh 'docker-compose down -d && docker-compose up -d'
            }
        }
    }
}

pipeline {
    agent any

    environment{
        DOCKER_IMAGE = 'lalbudha47/static-web-dashboard'
        CONTAINER_NAME = 'static-web-dashboard-container'
    }

    stages {
        stage ('Fetch Code') {
            steps{
                echo 'Cloning Code From Github'
                git url:'https://github.com/codeboylal/static-website-dashboard.git', branch: 'main'
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

        stage('Deploy') {
            steps {
                echo 'Deploying to container'
                sh "docker pull ${DOCKER_IMAGE}"
                
                echo 'Stopping previous container'
                sh "docker stop ${CONTAINER_NAME} || true"
                
                echo 'Removing previous container'
                sh "docker rm ${CONTAINER_NAME} || true"
                
                echo 'Starting new container'
                sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 ${DOCKER_IMAGE}"
            }
        }
    }
}

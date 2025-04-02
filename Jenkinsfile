pipeline {
    agent any
    
    stages {
        stage ('Fetch Code') {
            steps{
                echo 'Cloning Code From Github'
                git url:'https://github.com/codeboylal/static-website-dashboard.git', branch: 'devops'
            }
        }

        stage ('Building Code - Docker') {
            steps{
                echo 'Building Code to Docker'
                sh 'docker build -t my-static-image .'       
            }
        }

        stage ('Running Code - Docker') {
            steps{
                echo 'Running Code to Docker'
                sh '''
                    docker stop $(docker ps -q --filter ancestor=my-static-image) || true
                    docker rm $(docker ps -aq --filter ancestor=my-static-image) || true
                '''
                sh 'docker run -d -p 3030:80 my-static-image'
            }
        }
    }
}
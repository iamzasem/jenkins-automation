pipeline {
    agent any
    
    stages {
        stage ('Fetch Code') {
            steps{
                echo 'Cloning Code From Github'
                git url:'https://github.com/iamzasem/jenkins-automation/', branch: 'devops'
            }
        }

        stage ('Building Code - Docker') {
            steps{
                echo 'Building Code to Docker'
                sh 'sudo docker build -t my-static-image .'       
            }
        }


        // Deployment Stage
        stage ('Running Code - Docker') {
            steps{
                echo 'Running Code to Docker'
                sh '''
               sudo docker ps -q | xargs -r docker stop
                sudo docker ps -aq | xargs -r docker rm
                '''
                sh 'sudo docker run -d -p 3030:80 my-static-image'
            }
        }
    }
}
pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
        fullname = 'tnthach'
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo $fullname'
                echo 'Building nginx image..'
                sh 'docker build -t zerochkdocker/demo-image:1.0 .'
                
            }
        }
        stage('Pushing image') {
            steps {
                echo 'Start pushing.. with credential'
                sh 'echo $DOCKERHUB_CREDENTIALS'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push zerochkdocker/demo-image:1.0'
                
            }
        }
        stage('Deploying') {
            steps {
                echo 'Deploying....'
                sh 'docker image rm zerochkdocker/demo-image:1.0'
                sh 'docker container stop my-demo-nginx || echo "this container does not exist" '
                sh 'docker network create jenkins || echo "this network exists"'
                sh 'echo y | docker container prune '
                sh 'echo y | docker image prune'

                sh 'docker container run -d --rm --name my-demo-nginx -p 80:80 --network jenkins zerochkdocker/demo-image:1.0'
            }
        }
        
    }
}

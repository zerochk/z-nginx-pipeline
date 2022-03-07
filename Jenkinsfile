pipeline {
    agent any

    parameters {
        string(name: 'PERSON', defaultValue: 'Thach', description: 'Input your name')
        text(name: 'Introduction', defaultValue:'', description:'Share something about you')
        booleanParam(name:'Male', defaultValue:true)
        choice(name:'English-level', choices:['Lv1','Lv2','Lv3'],description:'select your english level')
    }
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
        fullname = 'tnthach'
    }
    stages {
        stage('Initialise') {
            steps {
                echo "Hello: ${params.PERSON}"
                echo "Introduction: ${params.Introduction}"
                echo "You are ${params.Male}"
                echo "English level ${params.English-level}"
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

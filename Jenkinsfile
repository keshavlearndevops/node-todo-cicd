pipeline {
    agent any
    environment {
        DOCKERHUB_CREDS = credentials('dockerHub')
    }
    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
                sh 'ls *'
            }
        }
        stage('Build Image') {
            steps {
                //sh 'docker build -t raj80dockerid/jenkinstest ./pushdockerimage/' (this will use the tag latest)
		        sh 'docker build -t keshavkumar23022000/nodeapp-new-test:$BUILD_NUMBER .'
            }
        }
        stage('Docker Login') {
            steps {
                //sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW' (this will leave the password visible)
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'  
                sh 'docker push keshavkumar23022000/nodeapp-new-test:$BUILD_NUMBER'              
                }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }    
    }    
}

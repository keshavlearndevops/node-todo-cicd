/* pipeline {
    agent {
	    label 'dev-agent'
    }
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
                // (this will use the tag latest)
		        sh 'docker build -t keshavkumar23022000/nodetodoapp-new-test .'
            }
        }
        stage('Docker Login') {
            steps {
                //sh 'docker login -u $DOCKERHUB_CREDS_USR -p $DOCKERHUB_CREDS_PSW' (this will leave the password visible)
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'  
                sh 'docker push keshavkumar23022000/nodetodoapp-new-test'              
                }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }    
    }    
}
*/
pipeline {
    agent {
        label 'dev-agent'
    }
    stages {
        stage('Code'){
            steps{
                git url:'https://github.com/keshavlearndevops/node-todo-cicd.git',branch:'master'
            }
        }
        stage('Build & Test'){
            steps{
                sh 'docker build -t my-node-application .'
            }
            
        }
        stage('Push to Repository'){
            steps{
                withCredentials([usernamePassword(credentialsId:'dockerHub', passwordVariable:'dockerHubPass',usernameVariable:'dockerHubUser')]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag my-node-application:latest ${env.dockerHubUser}/my-node-application:latest"
                    sh "docker push ${env.dockerHubUser}/my-node-application:latest"
                    
                }
            }
        }
        stage('Deploy'){
            steps{
                sh "docker-compose down"
                sh 'docker-compose up -d'
            }
 
        }
    }    
}

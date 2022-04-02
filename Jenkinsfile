pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/chiranjeevi36/jenkins-kubernetes-example.git']]])
            }
        }
        stage('Build docker image') {
            steps {
              script {
                  sh 'docker build -t chiru236/nodejsapp:1.0 .'
              }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push chiru236/nodejsapp:1.0'
            }
        }
        stage('Deploy App on k8s') {
      steps {
            sshagent(['deploy_to_k8s']) {
            sh "scp -o StrictHostKeyChecking=no nodejsapp.yaml ubuntu@172.31.13.254:/home/ubuntu"
            script {
                try{
                    sh "ssh ubuntu@172.31.13.254 kubectl create -f ."
                }catch(error){
                    sh "ssh ubuntu@172.31.13.254 kubectl create -f ."
            }
          }
        }
      }
    }
  }
}

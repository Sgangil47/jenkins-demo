pipeline {
    agent any
    environment{
      DOCKERHUB_CREDENTIALS=credentials('Docker-Hub')
    }

    stages {
        stage('git') {
            steps {
                echo '-----------------pulling code from source-------------------'
               git branch: 'main', credentialsId: 'GitLab-ssh', url: 'git@gitlab.com:sachin.gangil/jenkins-demo.git'
            }
        }
        stage('Initialize'){   
        def dockerHome = tool 'myDocker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
    }
        stage('Build') {
            steps {
                echo '----------------building image of source code---------'
                sh 'docker build -t 700085/jenkins-demo:latest .'
            }
        }
        stage('Login') {
            steps {
                echo '---------------------Logging in Docker-Hub to push the builded image--------------------'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                echo '--------------------------Image pushed to Docker-Hub successfully----------------'
                sh 'docker push 700085/jenkins-demo:latest'
            }
        }
    }
    post{
      always{
        sh 'docker logout'
      }
    }
}

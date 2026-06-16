@Library("docker") _
pipeline {
    agent any
    
    stages {
        
        stage('clone-code') {
            steps {
                script{
                    clone('https://github.com/Robinsingh200/flask-app.git' , 'main')
                }
            }
        }
        stage('build') {
            steps {
                sh 'docker build -t devops-app .'
                echo 'finished'
            }
        }
        stage('push-docker-hub') {
            steps {
                withCredentials([usernamePassword(credentialsId:'token_code'
                ,passwordVariable: 'dockerHubPass',
                usernameVariable: 'dockerUserHub')]){
                sh 'docker login -u $dockerUserHub -p $dockerHubPass'
                sh 'docker image tag devops-app $dockerUserHub/devops-app'
                sh 'docker push $dockerUserHub/devops-app '
                echo 'finished -docker hub'
             }
            }
        }
        stage('deploy') {
            steps {
                 sh 'docker run -d -p 80:80 devops-app'
                 echo 'finished -deploy'
                 sh 'echo $BUILD_NUMBER'
                sh 'echo $GIT_COMMIT'
                sh 'echo $JOB_NAME'
                sh 'echo $WORKSPACE'
                sh 'echo $JENKINS_HOME'
                sh 'echo $BRANCH_NAME'
            }
        }
    }
}

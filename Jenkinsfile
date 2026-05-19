pipeline {
    agent any
    environment { 
        TAG = sh (returnStdout: true, script: 'date "+%d%m%Y-%H%M%S"').trim()
    }
    stages {
        stage("Clone Git Repository") {
            steps {
                git(
                    url: "https://github.com/rafaam2002/devops_lab1",
                    branch: "main"
                )
            }
        }
        stage('Build') {
            steps {
                sh '''
                echo "Building..."
                docker build -t TU_USUARIO_DOCKER/flask_app:$TAG .
                docker tag TU_USUARIO_DOCKER/flask_app:$TAG rafaam02/flask_app:latest
                '''
            }
        }
        stage('Publish') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "Publishing..."
                        docker login -u="${USERNAME}" -p="${PASSWORD}"
                        docker push rafaam02/flask_app:$TAG
                    ''' 
                }
            }
        }
        stage('Clean') {
            steps {
                sh '''
                echo "Cleaning..."
                docker rmi rafaam02/flask_app:$TAG
                ''' 
           }
        }
    }
}

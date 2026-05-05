pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t snapchat-app .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred-id',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Docker Tag') {
            steps {
                sh 'docker tag snapchat-app sakit333/snapchat-app:latest'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push sakit333/snapchat-app:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f snapchat-container || true
                docker run -d -p 8084:8080 --name snapchat-container sakit333/snapchat-app:latest
                '''
            }
        }
    }
}

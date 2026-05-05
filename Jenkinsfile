pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/yashwanthmk11/snapchat_sak_cicd_docker_war.git'
            }
        }

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
                sh 'docker tag snapchat-app sakit333/snapchat-sak-cicd-docker:latest'
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push sakit333/snapchat-sak-cicd-docker:latest'
            }
        }

        stage('Cleanup') {
            steps {
                sh '''
                docker rmi sakit333/snapchat-sak-cicd-docker:latest || true
                docker rmi snapchat-app || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f snapchat-container || true
                docker run -d -p 8084:8080 --name snapchat-container sakit333/snapchat-sak-cicd-docker:latest
                '''
            }
        }
    }
}

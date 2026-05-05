pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/yashwanthmk11/snapchat_sak_cicd_docker_war.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t snapchat-app .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f snapchat-container || true
                docker run -d -p 8084:8080 --name snapchat-container snapchat-app
                '''
            }
        }
    }
}

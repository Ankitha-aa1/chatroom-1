pipeline {
    agent any
 stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Ankitha-aa1/chatroom-1.git'
            }
        }
        stage('validate code'){
            steps{
                sh 'mvn clean validate'
            }
        }
        stage('compilation'){
            steps{
                sh 'mvn compile'
            }
        }
    }
}
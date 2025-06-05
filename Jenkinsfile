pipeline {
    agent any
    tools{
        maven 'maven3'
    }
    environment{
        SONARQUBE_HOME = 'sonar-scanner'
    }
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
                sh 'mvn --version'
                sh 'mvn compile'
            }
        }
        stage('trivy fs'){
            steps{
                sh 'trivy fs -o file-scan.html .'
            }
        }
        stage('sonarqube analysis') {
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SONARQUBE_HOME/bin/sonar-scanner -Dsonar.projectName=chatroom-1 \
                    -Dsonar.projectKey=chatroom-1 -Dsonar.java.binaries=target '''
                }
            }
        }
        
    }
}
pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'Sonar-server'           
        SONARQUBE_TOKEN = credentials('sonar-token') 
        MAVEN_HOME = tool 'maven'                    
        TRIVY_SEVERITY = 'CRITICAL,HIGH'
        TOMCAT_WEBAPPS = '/usr/local/tomcat9/webapps'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Ankitha-aa1/chatroom-1.git', branch: 'main'
            }
        }

        stage('Validate') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn validate"
            }
        }

        stage('Compile') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn compile"
            }
        }

        stage('Trivy File Scan') {
            steps {
                sh "trivy fs . --severity ${TRIVY_SEVERITY} --exit-code 0 || true"
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv("

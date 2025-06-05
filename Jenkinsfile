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
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    sh "${MAVEN_HOME}/bin/mvn sonar:sonar -Dsonar.login=${SONARQUBE_TOKEN}"
                }
            }
        }

        stage('SonarQube Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn org.owasp:dependency-check-maven:check"
            }
        }

        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn package -DskipTests"
            }
        }

        stage('Delete Previous Deployment') {
            steps {
                sh "rm -rf ${TOMCAT_WEBAPPS}/chatroom*"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh "cp target/*.war ${TOMCAT_WEBAPPS}/chatroom.war"
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}

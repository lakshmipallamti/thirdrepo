pipeline {
    agent any
    environment {
        PATH="$PATH:/opt/maven3.8/bin"
    }
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/lakshmipallamti/thirdrepo.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('sonar') {
            steps {
                withSonarQubeEnv('sonarqube-8.9.6.50800') {
                    sh 'mvn sonar:sonar'
                }
            }       
        }
        stage('deployment') {
            steps {
                sshagent(['tomcat-server']) {
                    sh 'scp -o StrictHostKeyChecking=no target/in.maven.war ec2-user@13.232.8.240:/home/ec2-user/apache-tomcat-9.0.71/webapps'
                }
            }
        }
    }
}

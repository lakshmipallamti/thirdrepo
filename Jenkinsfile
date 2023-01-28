pipeline {
    agent any
    environment {
        PATH="$PATH:/opt/maven3.8/bin"
    }
    stages {
        stage('SCM') {
            steps {
                git'https://github.com/lakshmipallamti/thirdrepo.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('code review') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('deployment') {
            steps {
                sshagent(['tom']) {
                    sh 'scp -o StrictHostKeyChecking=no target/in.maven.war ec2-user@13.232.149.123:/home/ec2-user/apache-tomcat-9.0.71/webapps'
                }
            }
        }
        stage('artifact repo') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'in.maven', classifier: '', file: 'target/in.maven.war', type: 'war']], credentialsId: 'nexus', groupId: '01-maven-webapp', nexusUrl: '3.110.160.216:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'myrepository', version: '1.0-SNAPSHOT'
            }
        }
    }
}

pipeline {
    triggers {
      pollSCM '* * * * *'
    }
    agent {
      label 'windows && (oracle || mysql || postgresql)'
    }
    stages {
        stage('Pre Build') {
            steps {
                bat 'mvn clean'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn install'
            }
           post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('SonarQube analysis') {
            steps {
                    withSonarQubeEnv(credentialsId: 'f127f50b-56c3-48c4-88e4-8ab801e661d6', installationName: 'SonarQ Install') {
                    bat 'mvn clean package sonar:sonar'
                } 
            }
        }
            
 
    }
    
    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            mail bcc: '', body: 'test', cc: '', from: '', replyTo: '', subject: 'test', to: 'raphael.lample@gmail.com'
        }
    }
}

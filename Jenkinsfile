pipeline {
    triggers {
      pollSCM '* * * * *'
    }
    agent {
      label 'windows && (oracle || mysql || postgresql)'
    }
    stages {
        stage('Git') {
            steps {
                git branch: '**', credentialsId: '87641e3b-339c-4934-b712-b6ce02d88e2f', url: 'git@github.com:exahh/hello_world.git'
            }
        }
        stage('Build') {
            steps {
                git branch: '**', credentialsId: '87641e3b-339c-4934-b712-b6ce02d88e2f', url: 'git@github.com:exahh/hello_world.git'
                bat 'mvn clean install'
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

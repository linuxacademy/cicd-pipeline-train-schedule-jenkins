pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                
                git credentialsId: 'github', url: 'https://github.com/mailv1/cicd-pipeline-train-schedule-jenkins.git'
                echo 'Running build automation'
                sh 'pwd'
                sh 'pytest test1.py  --junitxml=report.xml'
        
            }
        }
    }
    
    post {
      always {
        junit '/var/jenkins_home/*/*.xml'
      }
   } 
}

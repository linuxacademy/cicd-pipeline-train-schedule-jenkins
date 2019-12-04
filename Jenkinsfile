pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh 'pwd'
                sh 'pytest test1.py  --junitxml=report.xml'
                sh 'sleep 420'
        
            }
        }
    }
    
    post {
      always {
        junit '**/*.xml'
      }
   } 
}

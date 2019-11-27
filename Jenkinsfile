pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                input "Does the staging environment look ok?"
                echo 'Running build automation'
                sh 'pwd'
                sh 'pytest test1.py  --junitxml=report.xml'
        
            }
        }
    }
    
    post {
      always {
        junit '*.xml'
      }
   } 
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
              echo "step1"     
        }
    }
    
    post {
      always {
        junit '*.xml'
      }
   } 
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'pwd'
                //sh 'git clone https://github.com/mailv1/cicd-pipeline-train-schedule-git.git'
                echo 'Running build automation'
                sh 'pwd'
                //sh 'sleep 420'
               //sh 'pytest test1.py  --junitxml=report.xml'
               echo $id
        
            }
        }
    }
    
    post {
      always {
        //sh 'rm -rf cicd-pipeline-train-schedule-git'  
        echo "post run"  
        //junit '/var/jenkins_home/*/*.xml'
        //new comment  
          
          
      }
   } 
}

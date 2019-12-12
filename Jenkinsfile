pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'pwd'
                //sh 'git clone https://github.com/mailv1/cicd-pipeline-train-schedule-git.git'
                echo 'Running build automation'
                sh 'pwd'
                //echo "${params.base} <<<<<<<<World!"
                echo "WORKSPACE--> $WORKSPACE"
                echo "JOB_NAME--> $JOB_NAME"
                echo "BUID_NUMBER--> $BUILD_NUMBER"
                echo "BRANCH_NAME--> $BRANCH_NAME"
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

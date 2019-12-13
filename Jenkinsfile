pipeline {
    agent any
    stages {
	stage('Build') {
            steps {
                sh 'pwd'
		//sh 'git clone https://github.com/mailv1/cicd-pipeline-train-schedule-git.git'
		//echo "${params.base}"
		echo 'Running build automation'
		sh 'pwd'
		/*echo "WORKSPACE--> $WORKSPACE"
		echo "JOB_NAME--> $JOB_NAME"
		echo "BUID_NUMBER--> $BUILD_NUMBER"
		echo "BRANCH_NAME--> $BRANCH_NAME"*/
		sh "printenv | sort"    
	    }
	
	}
    }
  
    post {
      always {
	 emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n ------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "jenkins99019@gmail.com", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
       
	//sh 'rm -rf cicd-pipeline-train-schedule-git'
	echo "post run"
	//junit '/var/jenkins_home/*/*.xml'
	//new comment
      }
   }
}


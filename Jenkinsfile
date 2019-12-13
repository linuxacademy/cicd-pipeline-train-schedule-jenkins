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
		/*echo " GIT_AUTHOR_EMAIL--> $GIT_AUTHOR_EMAIL"
		echo "GIT_AUTHOR_NAME --> $GIT_AUTHOR_NAME"
		echo "GIT_COMMITTER_EMAIL --> $GIT_COMMITTER_EMAIL"
		echo "GIT_LOCAL_BRANCH --> $GIT_LOCAL_BRANCH"
		echo "GIT_BRANCH --> $GIT_BRANCH"*/
	    }
		steps {	
		def fields = env.getEnvironment();    
			    
		}    
			    
			    
		    
	     /*   script {
                   def fields = env.getEnvironment()
                   fields.each {
			key, value -> println("${key} = ${value}");
                    }

                    println(env.PATH)
            }*/
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


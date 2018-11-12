pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
		sh './gradlew build --no-daemon'
            }
        }
	stage('Archive Artifact') {
            steps {
                echo 'Archive Artifact'
		archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
		steps {
		    echo 'Docker Image'
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
		echo 'Docker Push'
            }
        }
        stage('CanaryDeploy') {
            steps {
		echo 'Canary Deploy'
            }
        }
        stage('DeployToProduction') {
	    steps {
                input 'Deploy to Production?'
		echo 'Production Deploy'

            }
        }
    }
}

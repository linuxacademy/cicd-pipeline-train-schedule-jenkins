pipeline {
    agent any

    stages {
        stage('Build') 
		{
            steps {
                echo 'Running build automation'
            }
        }
		        stage('Archive Artifact') 
		{
            steps {
                echo 'Archive Artifact'
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

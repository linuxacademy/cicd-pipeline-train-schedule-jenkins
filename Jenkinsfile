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
	stage('DeployToStaging') {
	    when {
                branch 'master'
            }
            steps {
		echo 'Deploy To Staging'
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
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

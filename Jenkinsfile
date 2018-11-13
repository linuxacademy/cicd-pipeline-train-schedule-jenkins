pipeline {
    agent any
    environment {
    	MAJOR_VERSION = 1
    }

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
	    
	stage('Promote Development Branch to Master') {
	      agent any
	      when {
		branch 'development'
	      }
	      steps {
		echo "Stashing Any Local Changes"
		sh 'git stash'
		echo "Checking Out Development Branch"
		sh 'git checkout development'
		echo 'Checking Out Master Branch'
		sh 'git pull origin'
		sh 'git checkout master'
		echo 'Merging Development into Master Branch'
		sh 'git merge development'
		echo 'Pushing to Origin Master'
		sh 'git push origin master'
		echo 'Tagging the Release'
		sh "git tag rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
		sh "git push origin rectangle-${env.MAJOR_VERSION}.${env.BUILD_NUMBER}"
	      }
	      post {
		success {
		  emailext(
		    subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Development Promoted to Master",
		    body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Development Promoted to Master":</p>
		    <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
		    to: "pradeep.edirisinghe@accenture.com"
		  )
		}
	      }
	    }
	  }
	
	stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("pradeepe/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
	        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DockerHub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
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
	
	stage('DeployToProduction') {
	    when {
                branch 'master'
            }
            steps {
		input 'Deploy to Production?'
		echo 'Production Deploy'
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
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
	post {
	  failure {
	    emailext(
		subject: "${env.JOB_NAME} [${env.BUILD_NUMBER}] Failed!",
		body: """<p>'${env.JOB_NAME} [${env.BUILD_NUMBER}]' Failed!":</p>
		<p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
		to: "pradeep.edirisinghe@accenture.com"
	    )
	  }
	}       
    }
}

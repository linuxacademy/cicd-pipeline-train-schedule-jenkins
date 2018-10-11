pipeline {
    agent any
    stages {
       stage('checkout') {
           steps {
               git clone "git@github.com:ericn-clover/cicd-pipeline-train-schedule-jenkins.git"
           }
       }

        stage('Build') {
            steps {
                "./gradlew build"
            }
        }
        stage('archive') {
            steps {
                // One or more steps need to be included within the steps block.
            }

            post {
                success {
                    // One or more steps need to be included within each condition's block.
                }
            }
        }

        stage('Deploy') {
            steps {
                //
            }
        }
    }
}
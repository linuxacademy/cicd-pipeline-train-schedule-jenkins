pipeline {
  agent any
  Stages {
    stage ('Build') {
      steps { 
        echo 'Running build automatic'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
        }
       } 
      }
    }

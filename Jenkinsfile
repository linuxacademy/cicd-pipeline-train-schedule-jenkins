pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        echo 'Running build automation'
        archiveArtifacts artifacts: 'train-schedule/trainSchedule.zip'
      }
    }
  }
}

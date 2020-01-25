pipeline {
  agent any
  stages {
     stage('Build'){
       steps{
         echo 'Running build Automation'
         sh './gradlew build --no-daemon'
         archiveArtifacts artifacts: 'dist/trainSchedule.zip'
       }   
     }
  }
}

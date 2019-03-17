
pipeline{
  agent any
  stages{
    stage('build'){
      steps {
        echo "Running build"
        sh'./gradelew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
      }
      
    }
  }
}

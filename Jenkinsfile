pipeline {
  agent any
  environment {
    DOCKER_IMAGE_NAME = "dafespinelsa/train-schedule"
  }
  stages {
    stage('Build') {
      steps {
        echo 'Running build automation'
        sh './gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
      }
    }
    stage('Build Docker Image') {
      when {
        branch 'DEV-feature-jenkins'
      }
      steps {
        script {
          echo 'Building Docker image'
          app = docker.build("dafespinelsa/train-schedule")
          app.inside {
            sh 'echo $(curl localhost:3000)'
          }
        }
      }
    }
    stage('Push Docker Image') {
      when {
        branch 'DEV-feature-jenkins'
      }
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_registry') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
          }
        }
      }
    }
    stage('DeployToStaging') {
      when {
        branch 'DEV-feature-jenkins'
      }
      steps {
        kubernetesDeploy(
          kubeconfigId: 'kube-stage',
          configs: 'train-schedule-kube.yml',
          enableConfigSubstitution: true
        )
      }
    }
    stage('DeployToProduction') {
      when {
        branch 'DEV-feature-jenkins'
      }
      steps {
        input 'Does the staging environment look OK?'
        milestone(1)
        kubernetesDeploy(
          kubeconfigId: 'kube-prod',
          configs: 'train-schedule-kube.yml',
          enableConfigSubstitution: true
        )  
        
      }
    }
  }
}

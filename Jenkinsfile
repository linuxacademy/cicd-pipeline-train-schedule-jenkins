pipeline {
  agent any
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
        withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
          script {
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.stage_ip} \"docker pull dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
            try {
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.stage_ip} \"docker stop train-schedule\""
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.stage_ip} \"docker rm train-schedule\""
            } catch (err) {
              echo: 'caught error: $err'
            }
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.stage_ip} \"docker run --restart always --name train-schedule -p 3000:3000 -d dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
          }
        }
      }
    }
    stage('DeployToProduction') {
      when {
        branch 'DEV-feature-jenkins'
      }
      steps {
        input 'Does the staging environment look OK?'
        milestone(1)
        withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
          script {
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \"docker pull dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
            try {
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \"docker stop train-schedule\""
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \"docker rm train-schedule\""
            } catch (err) {
              echo: 'caught error: $err'
            }
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@${env.prod_ip} \"docker run --restart always --name train-schedule -p 3000:3000 -d dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
          }
        }
      }
    }
  }
}

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
        branch 'master'
      }
      steps {
        script {
          app = docker.build("dafespinelsa/train-schedule")
          app.inside {
            sh 'echo $(curl localhost:3000)'
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
          docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_registry') {
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
        withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
          script {
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@stage_ip \"docker pull dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
            try {
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChacking=no $USERNAME@stage_ip \"docker stop train-schedule\""
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChacking=no $USERNAME@stage_ip \"docker rm train-schedule\""
            } catch (err) {
              echo: 'caught error: $err'
            }
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChacking=no $USERNAME@stage_ip \"docker run --restart-always --name train-schedule -p 3000:3000 -d dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
          }
      }
    }
    stage('DeployToProduction') {
      when {
        branch 'master'
      }
      steps {
        input 'Does the staging environment look OK?'
        milestone(1)
        withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
          script {
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@prod_ip \"docker pull dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
            try {
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChacking=no $USERNAME@prod_ip \"docker stop train-schedule\""
              sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChacking=no $USERNAME@prod_ip \"docker rm train-schedule\""
            } catch (err) {
              echo: 'caught error: $err'
            }
            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChacking=no $USERNAME@prod_ip \"docker run --restart-always --name train-schedule -p 3000:3000 -d dafespinelsa/train-schedule:${env:BUILD_NUMBER}\""
          }
        }
      }
    }
  }
}

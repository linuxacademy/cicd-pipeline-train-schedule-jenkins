pipeline {
    agent any
//     parameters {
//         string(defaultValue: "", description: 'BUILD_NUMBER', name: 'BUILD_NUMBER')
//         }
    stages {
        stage('Build') {
            steps {
            //sh export BUILD_NUMBER="${env.BUILD_NUMBER}"
            sh './gradlew build'
            }
        }
    }
    post {
        always {
            archieveArtifacts: artifacts: 'trainSchedule.zip', onlyIfSuccessful: true
        }
    }
}

pipeline {
    agent any
    parameters {
        string(defaultValue: "", description: 'BUILD_NUMBER', name: 'BUILD_NUMBER')
        }
    stages {
        stage('Build') {
            steps {
            sh './gradlew build'
            }
        }
}

pipeline {
    agent any
        parameters {
            string(defaultValue: "", description: 'BUILD_NUMBER', name: 'BUILD_NUMBER')
        }
    stages {
        stage('Checkout') {
        }
        stage('Build') {
            steps {
                script {
                    sh './gradlew build'
                    }
                }
        }

}
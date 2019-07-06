pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh 'pwd'
                sh 'sudo -u root pytest test1.py'
        
            }
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                input message: 'User input required', ok: 'Release!',
                parameters: [choice(name: 'RELEASE_SCOPE', choices: 'patch\nminor\nmajor', description: 'What is the release scope?')]
                echo "env: ${env.RELEASE_SCOPE}"
                echo "params: ${params.RELEASE_SCOPE}"
                echo 'Running build automation'
                sh 'pwd'
                sh 'pytest test1.py  --junitxml=report.xml'
        
            }
        }
    }
    
    post {
      always {
        junit '*.xml'
      }
   } 
}

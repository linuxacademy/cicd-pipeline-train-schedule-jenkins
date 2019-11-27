pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                            def userInput = input(id: 'userInput', message: 'Merge to?',
                            parameters: [[$class: 'ChoiceParameterDefinition', defaultValue: 'strDef', 
                            description:'describing choices', name:'nameChoice', choices: "QA\nUAT\nProduction\nDevelop\nMaster"]
                            ])

                            println(userInput); //Use this value to branch to different logic if needed
                }
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

pipeline {

    agent any

    stages {

        stage("Interactive_Input") {
            steps {
                script {

                    
                    // Variables for input
                    def inputConfig
                    def inputTest

                    // Get the input
                    def userInput = input(
                            id: 'userInput', message: 'Enter the repository for smoke test:?',
                            parameters: [

                                    string(defaultValue: 'git@github.com:repo1/regression.git',
                                            description: 'Clone with ssh link',
                                            name: 'Config'),
                            ])

                    // Save to variables. Default to empty string if not found.
                    inputConfig = userInput.Config?:''
                    //inputTest = userInput.Test?:''

                    // Echo to console
                    echo("IQA Sheet Path: ${inputConfig}")
                    //echo("Test Info file path: ${inputTest}")
                    
                    sh 'pytest test1.py  --junitxml=report.xml'

                    // Write to file
                    //writeFile file: "inputData.txt", text: "Config=${inputConfig}\r\nTest=${inputTest}"

                    // Archive the file (or whatever you want to do with it)
                    //archiveArtifacts 'inputData.txt'
                }
            }
        }
    }
    
        post {
          always {
            junit '*.xml'
      }
   }
}

pipeline {
agent any
stages {
       stage('Archive build output') {
                      steps {
                      echo 'Running build automation'
                       
                        sh "mkdir -p output"
                         sh "zip output/archieved-output.zip *"
                      archiveArtifacts artifacts: 'output/archieved-output.zip'
            }
}
}
}

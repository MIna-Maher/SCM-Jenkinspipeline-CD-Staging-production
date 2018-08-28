pipeline {
agent any
stages {
       stage('Archive build output') {
                      steps {
                      echo 'Running build automation'
                      sh    "pwd"
                          //   sh "mkdir -p ArchievedDir"
                      archiveArtifacts artifacts: '*'
            }
}
}
}

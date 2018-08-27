pipeline {
agent any
stages {
       stage('Build') {
                      steps {
                      echo 'Running build automation'
                      sh    "pwd"
                             sh "mkdir -p ArchievedDir"
                      archiveArtifacts artifacts: '.'
            }
}
}
}

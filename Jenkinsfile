pipeline {
agent any
/* Building Stage */    // CI Build stage 
       stages {                 
       stage('Archive build output') {
                      steps {
                      echo 'Running build automation'
                       
                        sh "mkdir -p output"
                         sh "zip output/archived-output.zip *"
                      archiveArtifacts artifacts: 'output/archived-output.zip' //output will be in output/archived-output.zip
            }
}
/* stage of Deploying to Stage Seb Server */
          stage('Deploying to stage web server') {
            when {                 /*It's the Running stage condition, this stage will be running in case of meeting the condition*/
                
                branch 'master'   // only in master branch 
}
                 steps {
 withCredentials([usernamePassword(credentialsId: 'web_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(  //using of publish over ssh plugin to move source code files to stage web server over ssh
                           failOnError: true,
                          continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'output/archived-output.zip',
                                        removePrefix: 'output/',
                                        remoteDirectory: '/tmp',
                                       // execCommand: 'rm -rf /opt/archived-output.zip/*'
                                        //   execCOmmand 'unzip /tmp/archieved-output.zip /var/www/html/'
                                        //   execCommand 'sudo /bin/systemctl restart httpd'
                                 )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}    


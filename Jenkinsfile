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
                                    //    removePrefix: 'output/',
                                        remoteDirectory: '/tmp',
                                    
                           execCommand: 'sudo unzip /tmp/output/archived-output.zip -d /tmp/output/ && sudo rm -rf /var/www/html/index.html && sudo mv /tmp/output/index.html /var/www/html/ && sudo chown -R apache:apache /var/www/html/ && sudo rm -rf /tmp/output/ && sudo systemctl restart httpd'
                                           
                                           
                                           
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
}

/* stage of Deploying to PRODUCTION Seb Server */
          stage('Deploying to Production web server FINAL') {
            when {                 /*It's the Running stage condition, this stage will be running in case of meeting the condition*/
                
                branch 'master'   // only in master branch 
}
                 steps {
                        input 'Do You want to Deploy to production Web Server ??? '
                         milestone(1)
                        
 withCredentials([usernamePassword(credentialsId: 'web_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(  //using of publish over ssh plugin to move source code files to stage web server over ssh
                           failOnError: true,
                          continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'output/archived-output.zip',
                                    //    removePrefix: 'output/',
                                        remoteDirectory: '/tmp',
                                    
                           execCommand: 'sudo unzip /tmp/output/archived-output.zip -d /tmp/output/ && sudo rm -rf /var/www/html/index.html && sudo mv /tmp/output/index.html /var/www/html/ && sudo chown -R apache:apache /var/www/html/ && sudo rm -rf /tmp/output/ && sudo systemctl restart httpd'
                                           
                                           
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

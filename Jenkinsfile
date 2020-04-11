
pipeline {
    agent any
    stages {
        stage ('BUILD_STAGE') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    
        stage ('TESTING_STAGE') {

            steps {
                sh 'mvn test'
                archiveArtifacts artifacts: 'target/*.jar'
              
            } 
        }
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDes(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'target/my-app-1.0-SNAPSHOT.jar',
                                        removePrefix: 'target/',
                                        remoteDirectory: '/tmp',
                                        
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

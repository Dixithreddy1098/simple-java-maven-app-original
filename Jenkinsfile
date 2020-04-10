
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
        stage ('DEPLOYING_TO_STAGING) {
            steps { 
                publishOverSsh { 
                    continueOnError = false
                    failOnError = true
                    server ('staging') {
                        credentials('staging') { 
                            username: 'cicd'
                            password: 'Siddhu1098'
                        label('staging')
                        transferSet { 
                            sourceFiles: 'target/my-app-1.0-SNAPSHOT.jar'
                            removePrefix: 'target/'
                            remoteDirectory: '/tmp'   
                        } 
                        } 
                    } 
                } 
             }                                   
        }

    }
}

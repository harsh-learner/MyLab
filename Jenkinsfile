pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
        ArtifactID = readMavenPom().getartifactID()
        GroupID = readMavenPom().getgroupID()
        Version = readMavenPom().getversion()
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

       // Stage3 : Publish to Nexus
          stage('Publishing'){
              steps{
                  nexusArtifactUploader artifacts: 
                      [[artifactId: '${ArtifactID}', 
                        classifier: '', 
                        file: 'target/VinayDevOpsLab-0.0.3-SNAPSHOT.war', 
                        type: 'war']], 
                        credentialsId: 'nexus', 
                        groupId: '${GroupID}', 
                        nexusUrl: '65.0.178.13:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'Harshlab-Snapshot', 
                        version: '${Version}'

              }
          }

        
        
    }

}

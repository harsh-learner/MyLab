pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        GroupId = readMavenPom().getGroupId()
        Version = readMavenPom().getVersion()
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
                  script{
                  def NexusRepo = Version.endsWith("Snapshot") ? "HarshLab-Snapshot" : "HarshLab-Release"
                  
                  nexusArtifactUploader artifacts: 
                      [[artifactId: "${ArtifactId}", 
                        classifier: '', 
                        file: "target/${ArtifactId}-${Version}.war", 
                        type: 'war']], 
                        credentialsId: 'nexus', 
                        groupId: "${GroupId}", 
                        nexusUrl: '52.66.239.222:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: "${NexusRepo}", 
                        version: "${Version}"

                  }
              }
          }
        // Stage4: Deployment to Tomcat
           stage('Deployment'){
               steps{
                   sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible-controller', 
                   transfers: [sshTransfer(cleanRemote: false, excludes: '', 
                   execCommand: '''ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts''', 
                   execTimeout: 120000, 
                   flatten: false, 
                   makeEmptyDirs: false, 
                   noDefaultExcludes: false, 
                   patternSeparator: '[, ]+', 
                   remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
               }
           }
               
                   

        
        
    }

}

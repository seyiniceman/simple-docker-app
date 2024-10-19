  
def remote = [:]
  remote.name = 'Docker Server'
  remote.host = '34.246.135.95'
  remote.user = 'ubuntu'
  remote.password = 'lincon'
  remote.allowAnyHosts = true

pipeline {
  agent any

  environment {
       imagename = "seyiniceman/zenithproject"
       registryCredential = 'Docker_ID'
       dockerImage = ''
       imagetag    = "${env.BUILD_ID}"
           }

     stages {

          stage('Building Docker image') {
               steps{
                   script {
                       dockerImage = docker.build imagename
                          }
               }
          }

          stage('Push Image To DockerHub') {
               steps{
                     script {
                         docker.withRegistry( '', registryCredential ) {
                         //dockerImage.push("$BUILD_NUMBER")
                         dockerImage.push("$imagetag")
                                              }
                         }
               }
          }

          stage('Deploy To Docker Server Using SSH') {
               steps{
                    script {
                         sshCommand remote: remote, command: "docker run --name olivia-docker-class3 -d -p 5050:80 seyiniceman/zenithproject:latest"
                    }
               }
          }

          stage('Remove Unused docker image') {
               steps{
                    //sh "docker rmi $imagename:$BUILD_NUMBER"
                    //sh "docker rmi $imagename:$imagetag"
                    sh "docker system prune -f"
                    }
          }
    
     }
}

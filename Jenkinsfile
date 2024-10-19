  
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
                         sshCommand remote: remote, command: "docker run --name olivia-docker-class2 -d -p 9090:80 seyiniceman/zenithproject-class:3"
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

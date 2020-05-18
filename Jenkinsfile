node{
  stage('SCM Checkout'){
      git credentialsId: 'git-creds', url: 'https://github.com/Balaji024/NodeJs-Repo.git', branch: 'master'
  }
  //stage('Fix The Permission Issue'){
    //  sh label: '', script: 'sudo chown root:jenkins /run/docker.sock', ttyEnabled: 'true'
  //}
  stage('Building Docker Image of NodeJS Application'){
     sh "docker build -t balaji024/cr7-blog:${BUILD_NUMBER} ."
  }
  stage('Pushing the image to Docker Repo'){
      // This step should not normally be used in your script. Consult the inline help for details.
       withCredentials([string(credentialsId: 'docpass3', variable: 'dockerpass1')]) {
          sh """
               docker login -u balaji024 -p ${dockerpass1}
               docker tag balaji024/cr7-blog:${BUILD_NUMBER} balaji024/cr7-blog:${BUILD_NUMBER}
               docker push balaji024/cr7-blog:${BUILD_NUMBER}
              """
       }
      
  }
  stage('Running Application in Container'){
    withCredentials([string(credentialsId: 'docpass3', variable: 'dockerpass1')]){
      sh label: '', script: '''#!/bin/bash
                              if (docker ps | grep 9678)
                              then
                                docker stop $(docker ps | grep 9678| awk '{print$1}')
                                docker login -u balaji024 -p ${dockerpass1}
                                docker pull balaji024/cr7-blog:${BUILD_NUMBER}
                                docker run -d -p 9678:8081 balaji024/cr7-blog:${BUILD_NUMBER}
                              else
                                docker login -u balaji024 -p ${dockerpass1}
                                docker pull balaji024/cr7-blog:${BUILD_NUMBER}
                                docker run -d -p 9678:8081 balaji024/cr7-blog:${BUILD_NUMBER}
                              fi
                            '''
       }
    }
}

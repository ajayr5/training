node {
  checkout scm
  stage('Package') {
      sh 'bash build.sh'
  }

  stage('Create Docker Image') {
    dir('webapp') {
      docker.build("ajayr5/dry-run:${env.BUILD_NUMBER}")
    }
  }

  
  stage('Push Docker Image') {
    dir('webapp') {
         withDockerRegistry(credentialsId: 'docker', url: '') {
         sh "docker push ajayr5/dry-run:${env.BUILD_NUMBER} "
         }
       }
    } 
}

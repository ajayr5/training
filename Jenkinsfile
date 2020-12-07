node {
  checkout scm
  stage('Package') {
      sh 'bash build.sh'
  }

  stage('Create Docker Image') {
      docker.build("ajayr5/dry-run:latest")
  }

  
  stage('Push Docker Image') {
         withDockerRegistry(credentialsId: 'docker', url: '') {
         sh "docker push ajayr5/dry-run:latest "
         
       }
    }
}

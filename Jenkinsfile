node {
  checkout scm
  // env.PATH = "${tool 'Maven3'}/bin:${env.PATH}"
  stage('Package') {
      sh 'go build -o main main.go'
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

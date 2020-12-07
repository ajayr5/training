node {
  checkout scm
  // env.PATH = "${tool 'Maven3'}/bin:${env.PATH}"
  stage('Setup') {
      sh '''
      export GOPATH=$WORKSPACE
      mkdir -p $GOPATH/src 
      ln -f -s $WORKSPACE $GOPATH/src/main
      go get main
      CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main main'''
}
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

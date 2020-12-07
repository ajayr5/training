node {
  checkout scm
  stage('Package') {
      sh 'bash build.sh'
  }

  stage('Create Docker Image') {
      docker.build("ajayr5/dry-run:${env.BUILD_NUMBER}")
  }

  
  stage('Push Docker Image') {
         withDockerRegistry(credentialsId: 'docker', url: '') {
         sh "docker push ajayr5/dry-run:${env.BUILD_NUMBER} "
         
       }
    }
  stage('Update Manifests') {
        sh '''
        curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
        mv kustomize /usr/local/bin/
        cd deployment/dev/
        kustomize edit set image "ajayr5/dry-run:$tag${env.BUILD_NUMBER}"
        git add .
        git commit -m "updating dev image tag in kustomization file"
        git push https://ajayr5:78Q6wa3s0%@github.com/ajayr5/training.git main
        ''' 
    
  }
}

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
      steps {
        git credentialsId: 'c98b21ef-e6cf-4d67-b3c2-18e09f39c9df', url: 'https://github.com/sladedge/LMS-Image-Denoising-Framework_deployment.git', branch: 'main'
        sh '''
        cd deployment/dev/
        kustomize edit set image ajayr5/dry-run:${env.BUILD_NUMBER}
        git add .
        git commit -m "updating dev image tag in kustomization file"
        git push https://ajayr5:78Q6wa3s0%@github.com/ajayr5/training.git main
        '''
      }
    }
  }
}

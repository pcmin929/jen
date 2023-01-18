pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/pcmin929/jen.git', branch: 'main'
      }
    }
    stage('docker build') {
      steps {
        sh '''
        sudo echo $BUILD_NUMBER
        sudo docker build -t rapa.iptime.org:5000/mynginx:${BUILD_NUMBER} .
        sudo docker push rapa.iptime.org:5000/mynginx:${BUILD_NUMBER}
        '''
      }
    }
    
    stage('K8S Manifest Update') {
      steps {
        git credentialsId: '{git_pcmin929}',
              url: 'https://github.com/pcmin929/jen',
              branch: 'master'
        
        sh sudo sed "s/IMAGE_VERSION/${BUILD_NUMBER}/g" np-pod.yml > np-pod-deploy.yml
        sh "git add np-pod-deploy.yml"
        sh "git commit -m '[UPDATE] mynginx:{BUILD_NUMBER} image versioning'"
        sh "git remote set-url origin https://github.com/pcmin929/jen.git"
        sh "git push -u origin main"
             }
    }
    stage('deploy k8s') {
      steps {
        sh '''
        
        sudo kubectl apply -f np-pod-deploy.yml
        
        '''
      }
    }
    
  }
}

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
    
    stage('deploy k8s') {
      steps {
        sh '''
        sudo echo $BUILD_NUMBER
        sudo sed -e 's/IMAGE_VERSION/$BUILD_NUMBER/g' np-pod.yml > np-pod-deploy.yml
        sudo kubectl apply -f np-pod-deploy.yml
        
        '''
      }
    }
    
  }
}

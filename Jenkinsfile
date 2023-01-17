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
        sudo docker build -t rapa.iptime.org:5000/mynginx:${BUILD_NUMBER} .
        sudo docker push rapa.iptime.org:5000/mynginx:${BUILD_NUMBER}
        '''
      }
    }
    
    stage('deploy k8s') {
      steps {
        sh '''
        sudo sed 's/IMAGE_VERSION/${BUILD_NUMBER}/g' np-pod.yaml > np-pod-deploy.yaml
        sudo kubectl apply -f np-pod-deploy.yml
        sudo rm -rf np-pod-deploy.yml
        '''
      }
    }
    
  }
}

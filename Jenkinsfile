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
        sudo docker build -t rapa.iptime.org:5000/mynginx:${env.BUILD_NUMBER} .
        sudo docker push rapa.iptime.org:5000/mynginx:${env.BUILD_NUMBER}
        '''
      }
    }
    
    stage('deploy k8s') {
      steps {
        sh '''
        sudo kubectl apply -f np-pod.yml
        '''
      }
    }
    
  }
}

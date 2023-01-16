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
        docker build -t rapa.iptime.org:5000/mynginx .
        '''
      }
    }
    stage('deploy k8s') {
      steps {
        sh '''
        kubectl apply -f np-pod.yml
        '''
      }
    }
  }
}

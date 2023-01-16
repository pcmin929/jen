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
        sudo docker build -t rapa.iptime.org:5000/mynginx:latest .
        sudo docker push rapa.iptime.org:5000/mynginx:latest
        '''
      }
    }
    
    stage('deploy k8s') {
      steps {
        sh '''
        sudo kubectl delete -f np-pod.yml
        sleep 10
        sudo kubectl apply -f np-pod.yml
        '''
      }
    }
    
  }
}

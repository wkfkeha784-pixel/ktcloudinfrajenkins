pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/wkfkeha784-pixel/ktcloudinfrajenkins.git', branch: 'main'
      }
    }
    stage('delivery and deployment using k8s') {
      steps {
        sh '''
        ansible master -m shell -a "kubectl --kubeconfig=/etc/kubernetes/admin.conf get no"
        '''
      }
    }
  }
}

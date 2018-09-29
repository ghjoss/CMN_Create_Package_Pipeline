pipeline {
  agent {
    node {
      label 'RRMac_zOS2_SSH'
    }

  }
  stages {
    stage('1') {
      steps {
        sh 'printenv'
      }
    }
  }
  environment {
    WS = '/u/CMN/jenkins/pipelines/CrePkgPipe'
  }
}
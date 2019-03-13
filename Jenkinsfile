pipeline {

  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  
  agent {
    node {
      label 'docker'
    }
  }

  stages {
    stage('Checkout source') {
      steps {
        checkout scm
      }
    }
    
    stage('Build Docker') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'docker_hub') {
            def app = docker.build "thirdchannel/localtunnel-server:${env.BRANCH_NAME}"
            def head = sh(
              script: 'git rev-parse HEAD',
                          returnStdout: true
            ).trim()
            app.push head
          }
        }
      }
    }
  }
}

pipeline {
  agent any
  options { skipDefaultCheckout(true) }  // we do an explicit checkout
  stages {
    stage('Checkout') {
      steps {
        // Use the REPO URL, not a file-edit URL
        git branch: 'main', url: 'https://github.com/Muho003/MuhosDocker.git'
      }
    }
    stage('Preflight') {
      steps {
        sh 'docker --version'
        sh 'docker compose version || true'
      }
    }
    stage('Build') {
      steps {
        // This expects a docker-compose.yml in the repo root
        sh 'docker compose build'
      }
    }
    stage('Up') {
      when { expression { fileExists('docker-compose.yml') } }
      steps {
        sh 'docker compose up -d'
      }
    }
  }
  post {
    always {
      sh 'docker ps -a || true'
      archiveArtifacts artifacts: '**/logs/*.log', allowEmptyArchive: true
    }
  }
}


pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        echo 'Iniciando a build'
        sh 'cd \'/var/www/html\''
      }
    }

    stage('Build') {
      steps {
        sh '''git pull
php composer install'''
      }
    }

  }
}
pipeline {
  agent any
  stages {
    stage('Iniciando') {
      steps {
        echo 'Iniciando a build'
        sh 'cd \'/var/www/html\''
      }
    }

    stage('Construindo depend�ncias') {
      steps {
        sh '''git pull
php composer install'''
      }
    }

  }
}
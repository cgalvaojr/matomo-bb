pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        echo 'Iniciando a build'
        ws(dir: '/home/jenkins/workspace')
      }
    }

    stage('Build') {
      steps {
        sh '''docker-compose up -d
docker exec matomo-tests-php-fpm php composer install'''
      }
    }

  }
}
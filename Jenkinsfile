pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        ws(dir: '/home/jenkins/workspace')
        echo 'Iniciando a build'
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
pipeline {
  agent any
  stages {
    stage('Iniciando') {
      steps {
        echo 'Iniciando a build'
        sh 'cd \'/var/www/html\''
      }
    }

    stage('Construindo dependÃªncias') {
      steps {
        sh 'php /bin/composer install'
      }
    }

    stage('Testes unitarios') {
      steps {
        sh 'cd /var/www/html'
        sh 'php console tests:run unit'
      }
    }

    stage('error') {
      steps {
        sh 'php console tests:run integration'
      }
    }

  }
}
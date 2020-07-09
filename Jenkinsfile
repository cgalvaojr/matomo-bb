pipeline {
  agent any
  stages {
    stage('Iniciando') {
      steps {
        echo 'Iniciando a build'
        sh 'cd \'/var/www/html\''
      }
    }

    stage('Construindo dependências') {
      steps {
        sh 'php /bin/composer install'
      }
    }

    stage('Testes unit�rios') {
      steps {
        dir(path: '/var/www/html') {
          sh 'php console tests:run unit'
        }

      }
    }

    stage('') {
      steps {
        sh 'php console tests:run integration'
      }
    }

  }
}
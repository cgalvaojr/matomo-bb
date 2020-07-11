pipeline {
  agent any
  stages {
    stage('Iniciando') {
      steps {
        echo 'Iniciando a build'
        sh 'cd \'/var/www/html\''
      }
    }

    stage('Construindo dependencias') {
      steps {
        sh 'php /bin/composer install'
      }
    }

    stage('Testes unitarios') {
      steps {
        sh 'cd /var/www/html'
        sh 'php console development:enable'
        sh 'ls -lah'
        sh 'php console tests:run ArchiverTests'
      }
    }

    stage('Testes de integração') {
      steps {
        sh 'php console tests:run ActionSiteSearchTest'
      }
    }

    stage('Sonar') {
      steps {
        withSonarQubeEnv(installationName: 'jenkins-matomo', credentialsId: 'd8dd2174cfd8a7d7fbf0c3c9f9447084e4b4853f')
      }
    }

  }
}
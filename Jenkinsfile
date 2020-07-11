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
        sh '''sonar-scanner \\
  -Dsonar.projectKey=jenkins-matomo \\
  -Dsonar.sources=. \\
  -Dsonar.host.url=http://jenkins.local:9000 \\
  -Dsonar.login=2ea7f96b45f2a0ca30ef91f3dd7154132f03fd55'''
      }
    }

  }
}
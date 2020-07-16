pipeline {
  agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
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
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube Server') {
          sh 'mvn clean package sonar:sonar'
        }

      }
    }

  }
}

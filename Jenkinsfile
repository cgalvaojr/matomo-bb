pipeline {
  agent any
  stages {
    stage('Clonando repositorio') {
      steps {
        sh 'cd /var/www/html'
      }
    }

    stage('Construindo dependencias') {
      steps {
        sh '''curl -sS https://getcomposer.org/installer | php &&
php composer.phar install &&
php console git:pull'''
        sh 'php /bin/composer self-update > /dev/null'
        sh 'php /bin/composer install # --no-dev > /dev/null'
        sh '#php console core:update --yes > /dev/null'
        sh 'php console custom-piwik-js:update > /dev/null'
      }
    }

    stage('Testes unitarios') {
      steps {
        sh 'php console development:enable'
        sh 'php console tests:run VisitTime unit'
      }
    }

    stage('Testes de integracao') {
      steps {
        sh './console tests:run ActionSiteSearchTest integration'
      }
    }

    stage('Analise Sonar') {
      steps {
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube Server') {
          sh '/var/jenkins_home/sonnar-scanner-4.4.0.2170/bin/sonar-scanner -Dsonar.projectKey=matomo -Dsonar.sources=/var/jenkins_home/workspace/matomo-bb_master/plugins/Actions -Dsonar.host.url=http://192.168.0.19:9000 -Dsonar.login=admin -Dsonar.password=admin'
        }

      }
    }

    stage('Build') {
      environment {
        BUILD_NUMBER = ${env.BUILD_NUMBER}
      }
      steps {
        sh 'zip matomo-build-${env.BUILD_NUMBER}.zip /var/jenkins_home/workspace/matomo-bb_master'
      }
    }

  }
  environment {
    BUILD_NUMBER = '${BUILD_NUMBER}'
  }
}

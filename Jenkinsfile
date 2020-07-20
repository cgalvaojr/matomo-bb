pipeline {
  agent {
    any {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
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
        sh '#cd /var/www/html'
        sh 'php console development:enable'
        sh '#ls -lah'
        sh 'php console tests:run VisitTime unit'
      }
    }

    stage('Testes de integracao') {
      steps {
        sh './console tests:run CoreAdminHome integration'
      }
    }

    stage('Analise Sonar') {
      steps {
        withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'SonarQube Server') {
          sh '/var/jenkins_home/sonnar-scanner-4.4.0.2170/bin/sonar-scanner -Dsonar.projectKey=matomo -Dsonar.sources=/var/jenkins_home/workspace/matomo-bb_master/plugins/Actions -Dsonar.host.url=http://192.168.0.19:9000 -Dsonar.login=admin -Dsonar.password=admin'
        }

      }
    }

  }
}
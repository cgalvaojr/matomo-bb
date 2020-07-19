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
        sh '''cd \'/var/www/html\'
'''
        sh 'php console git:pull'
        sh 'php composer self-update > /dev/null'
        sh 'php composer install --no-dev > /dev/null'
        sh 'php console core:update --yes > /dev/null'
        sh 'php console custom-piwik-js:update > /dev/null'
      }
    }

    stage('Construindo dependencias') {
      steps {
        sh 'php /bin/composer install'
      }
    }

    stage('Testes unitarios') {
      steps {
        sh '#cd /var/www/html'
        sh 'php console development:enable'
        sh '#ls -lah'
        sh 'php console tests:run ArchiverTests unit'
      }
    }

    stage('Testes de integracao') {
      steps {
        sh 'php console tests:run ActionSiteSearchTest integration'
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
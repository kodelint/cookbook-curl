pipeline {
  agent any
  stages {
    stage('Setup') {
      steps {
        sh 'echo "Versions: "'
        sh 'chef --version'
        sh 'chef exec rubocop --version'
        sh 'chef exec foodcritic --version'
        sh 'echo "Updating Berkshelf: "'
        sh 'if [ ! -f Berksfile.lock ]; then chef exec berks install; else chef exec berks update; fi;'
      }
    }
    stage('Acceptance Testing') {
      parallel {
        stage('foodcritic') {
          steps {
            sh 'echo "Starting foodcritic: "'
            sh 'chef exec foodcritic .'
            sh 'bundle exec rake'
          }
        }
      }
    }
    stage('Test Kitchen') {
      steps {
        sh 'if [ ! -f Berksfile.lock ]; then chef exec berks install; else chef exec berks update; fi;'
      }
    }
  }
  post {
    success {
      echo 'Knife upload here'
      sh 'chef exec knife cookbook upload curl -o $WORKSPACE/cookbook-curl'

    }

    failure {
      echo 'The build failed'

    }

  }
}

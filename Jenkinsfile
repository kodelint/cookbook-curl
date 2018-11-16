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
            sh 'chef exec rake'
          }
        }
      }
    }
    stage('Upload Cookbook') {
      when {
        branch 'production'
      }
      steps {
        sh 'chef exec knife cookbook upload curl -o ../'
      }
    }
  }
  post {
    success {
      echo 'Build Passed'
      sh 'chef exec knife cookbook list'

    }
    failure {
      echo 'The build failed'

    }

  }
}

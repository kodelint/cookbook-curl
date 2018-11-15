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
      when {
          anyOf { branch 'master'; branch 'production' }
      }
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
    stage('Test Availibility of cummunity cookbooks') {
      steps {
        sh 'if [ ! -f Berksfile.lock ]; then chef exec berks install; else chef exec berks update; fi;'
      }
    }
    stage('\u27A1 Upload to Chef Server') {
      when {
          branch 'production'
      }
      steps { 
        sh 'chef exec knife cookbook upload curl -o ../'
      }
  }
  post {
    success {
      echo 'Successfully Uploaded the Cookbook to Chef Server'
      sh 'chef exec knife cookbook list'

    }
  }
}

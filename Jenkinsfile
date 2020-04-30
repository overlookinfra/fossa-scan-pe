pipeline {
  agent {
    label 'worker'
  }
  triggers {
    cron('@daily')
  }
  options {
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '50'))
    timeout(time: 8, unit: 'HOURS')

    // Works but in a later version than what we have; comment out for now
    // ansiColor('xterm')
  }
  environment {
    ARTIFACTORY_API_TOKEN = credentials('jenkins_artifactory_api_token')
  }
  stages {
    stage('FOSSA Analyze') {
      steps {
        sh './ci/build'
      }
    }
  }
  post {
    failure {
      slackSend channel: '#release-new-new',
        color: 'danger',
        message: "Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]. See: ${env.BUILD_URL}"
    }
    fixed {
      slackSend channel: '#release-new-new',
        color: 'good',
        message: "Repaired: ${env.JOB_NAME} [${env.BUILD_NUMBER}]. See: ${env.BUILD_URL}"
    }
  }
}

pipeline {
  agent {
    label 'k8s-worker'
  }
  triggers {
    cron('@daily')
  }
  options {
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '50'))
    timeout(time: 8, unit: 'HOURS')
    ansiColor('xterm')
  }
  environment {
    ARTIFACTORY_API_TOKEN = credentials('jenkins_artifactory_api_token')
    FOSSA_API_KEY = credentials('fossa_api_key')
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

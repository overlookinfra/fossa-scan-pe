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

    ansiColor('xterm')
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
}

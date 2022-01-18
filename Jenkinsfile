#!groovy

pipeline {
  agent none

  stages {

    // --- RELEASE ---
    stage('Release') {
      when {
        expression {
          GIT_TAG = sh(returnStdout: true, script: 'git describe --exact-match --tags HEAD || true')
          return GIT_TAG
          // return GIT_TAG ==~ /^(\d+\.)?(\d+\.)?(\*|\d+)$/
        }
      }
      steps {
        addShortText text: GIT_TAG, borderColor: 'transparent'
        addBadge icon: 'completed.gif', text: GIT_TAG
      }
    }
        stage("foo") {
            steps {
                echo "booleanExample: ${params.booleanExample}"
                echo "stringExample: ${params.stringExample}"
                echo "textExample: ${params.textExample}"
                echo "choiceExample: ${params.choiceExample}"
                echo "passwordExample: ${params.passwordExample}"
            }
        }
  }
}

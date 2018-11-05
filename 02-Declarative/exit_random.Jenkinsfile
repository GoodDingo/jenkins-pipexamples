#!groovy

def shWithFail(String script, int returnCode, String message)
{
  def shRetCode = sh(returnStatus: true, script: script)
  echo "Got exit code: '${shRetCode}'"
  if (shRetCode == returnCode) {
    currentBuild.result = 'UNSTABLE'
  } else if (shRetCode != 0) {
    currentBuild.result = 'FAILED'
    error(message)
  }
}


pipeline {
  agent any

  options {
    ansiColor('xterm')
    timestamps()
  }

  stages {
    stage("prvni shell") {
      steps {
        sh '''
          echo '\033[1;31m Hello... \033[0m'
          sleep 2
          echo '\033[1;34m JENKINS \033[0m'
          # exit 0
        '''.stripIndent()
      }
    }

    stage("druhy shell") {
      steps {
        shWithFail('''\
          #!/bin/bash
          echo "Druhy build step"
          exit $(( RANDOM % 3 ))
        '''.stripIndent(), 1, "Dostal jsem chybu - hned selhavam")
      }
    }

    stage("treti shell") {
      steps {
        sh ''' echo '\033[1;35m Treti build step \033[0m' '''
      }
    }
  }
}

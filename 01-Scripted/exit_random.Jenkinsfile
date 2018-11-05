#!groovy

node() {
  timestamps {
    stage ('prvni shell') {
      ansiColor('xterm') {
      sh '''
echo '\033[1;31m Hello... \033[0m'
sleep 2
echo '\033[1;34m JENKINS \033[0m'
# exit 0
'''
      }
    }

    stage ('druhy shell') {
      //druhy "Execute shell" build
      def shellReturnStatus = sh returnStatus: true, script: '''\
#!/bin/bash

echo "Druhy build step"
exit $(( RANDOM % 3 ))
'''
      if (shellReturnStatus == 1) {
        currentBuild.result = 'UNSTABLE'
      } else if (shellReturnStatus != 0) {
        currentBuild.result = 'FAILED'
        error("Dostal jsem chybu - hned selhavam")

      }
    }

    stage ('treti shell') {
      sh ''' echo '\033[1;35m Treti build step \033[0m' '''
    }
  }
}

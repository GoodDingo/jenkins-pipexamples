#!groovy

GIT_URL = 'http://3.121.42.212:80/gitserver/datascript/hello-world-examples'

pipeline {
  agent any

  options {
    ansiColor('xterm')
    timestamps()
    buildDiscarder logRotator(artifactDaysToKeepStr: '14', artifactNumToKeepStr: '100', daysToKeepStr: '60', numToKeepStr: '1000')
    skipStagesAfterUnstable()
  }

  stages {
    stage ('Checkout git') {
      steps {
        checkout([$class: 'GitSCM',
                  branches: [[name: 'develop']],
                  doGenerateSubmoduleConfigurations: false,
                  extensions: [],
                  submoduleCfg: [],
                  userRemoteConfigs: [
                    [credentialsId: '',
                    url: GIT_URL]
                  ]
                ])
      }
    }

    stage ('Build') {
      steps {
        withMaven(jdk: 'Java 8', maven: 'Maven 3') {
          sh "mvn -f 03-DropWizard--hello-demoapp/pom.xml clean package"
        }
      }
    }

    stage ('Verify version') {
      steps {
        sh """java -jar 03-DropWizard--hello-demoapp/target/demoapp.jar --version"""
      }
    }
  }

  post {
    always {
      junit('03-DropWizard--hello-demoapp/target/*-reports/*.xml')
    }
  }
}

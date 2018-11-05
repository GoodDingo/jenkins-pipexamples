#!groovy

GIT_URL = 'http://3.121.42.212:80/gitserver/datascript/hello-world-examples'


properties([
  buildDiscarder(
    logRotator(
      artifactDaysToKeepStr: '14',
      artifactNumToKeepStr: '100',
      daysToKeepStr: '60',
      numToKeepStr: '1000'))
  ])

node () {
	timestamps {

    stage ('Checkout git') {
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

    stage ('Build') {
      withEnv(["JAVA_HOME=${tool 'Java 8'}",
               "PATH=${tool 'Java 8'}/bin:${env.PATH}",
               "MAVEN_HOME=${tool 'Maven 3'}",
               "PATH+MAVEN=${tool 'Maven 3'}/bin",
               ]) {
      // withMaven(jdk: 'Java 8', maven: 'Maven 3') {
            sh "mvn -f 03-DropWizard--hello-demoapp/pom.xml clean package"
      }
    }

    stage ('Verify version') {
      sh """java -jar 03-DropWizard--hello-demoapp/target/demoapp.jar --version"""
    }

    stage ("Publish test results") {
      archiveArtifacts(allowEmptyArchive: false,
                      artifacts: '03-DropWizard--hello-demoapp/target/demoapp.jar',
                      caseSensitive: true,
                      defaultExcludes: true,
                      fingerprint: true,
                      onlyIfSuccessful: true)

      junit('03-DropWizard--hello-demoapp/target/*-reports/*.xml')
    }
	}
}

@Library('pipeline-library@master') _
pipeline {
  agent none
  options {
    timeout(time: 15, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '2'))
  }
  stages {
    stage("Import Catalog") {
      agent { label 'default-jnlp' } 
      when {
        branch 'master'
      }
      steps {
        withCredentials([usernamePassword(credentialsId: 'admin-cli-token', usernameVariable: 'JENKINS_CLI_USR', passwordVariable: 'JENKINS_CLI_PSW')]) {
          sh """
            curl -O http://teams-mycloudbeeig/teams-mycloudbeeig/jnlpJars/jenkins-cli.jar
            alias cli='java -jar jenkins-cli.jar -s http://teams-mycloudbeeig/teams-mycloudbeeig/ -auth $JENKINS_CLI_USR:$JENKINS_CLI_PSW'
            cli pipeline-template-catalogs --put < create-pipeline-template-catalog.json
          """
          pipelineCatalogLabCleanup('mycloudbeeig')
        }
      }
    }
  }
}

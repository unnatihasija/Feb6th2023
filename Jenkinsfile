@Library('piper-lib-os') _

pipeline {

    agent any
    parameters {
  choice choices: ['DEV', 'QAS', 'INT','PRD'], description: 'Select your ENV', name: 'DEPLOY_TO'
}
    stages {
        stage("prepare") {
            steps {
                deleteDir()
                checkout scm
                setupCommonPipelineEnvironment script: this
            }
        }
        stage('build') {
            steps {
                mtaBuild script: this
            }
        }
        stage("Deploy") {
            parallel {
                stage("DEV") {
                    when { expression { params.DEPLOY_TO == "DEV" } }
                    environment {
                         ORG = "CS"
                         SPACE = "PROD"
                         }
                    steps {
                        cloudFoundryDeploy script: this, deployTool:'mtaDeployPlugin'
                    }
                }
           
             }
         }
      }
   }


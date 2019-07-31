pipeline {
    agent any
    environment {
      CI = 'true'
      JENKINS_NODE_COOKIE = 'DoNotKill'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '1'))
        skipDefaultCheckout true
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/appsdemo/React-JS-Demo-ProductRecall.git']]])
                sh 'apk add nodejs'
            }
        }
        stage('Build') {
            steps {
              nodejs(nodeJSInstallationName: 'NodeJS 12.7.0') {
                sh 'npm install'
              }
            }
        }
        stage('Test') {
            steps {
              nodejs(nodeJSInstallationName: 'NodeJS 12.7.0') {

                sh 'npm run test'
              }
            }
        }
        stage('Deploy') {
            steps {
              script{
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']) {
                  nodejs(nodeJSInstallationName: 'NodeJS 12.7.0') {
                      sh 'su jenkins'
                      sh 'ssh ec2-user@52.66.247.248 "cd React-JS-Demo-ProductRecall && git reset --hard HEAD && git pull && ls && npm start"'
                  }
                }
              }
            }
        }
    }
}

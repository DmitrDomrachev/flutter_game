pipeline {

    options {
        buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '30'))
        disableResume()
    }

    agent any
    parameters {
        string(name: 'sourceBranch',      defaultValue: '', description: 'Ветка с pr, обязательный параметр')
        string(name: 'destinationBranch', defaultValue: '', description: 'Ветка, в которую будет мержиться пр, обязательный параметр')
    }

    environment {
        FLUTTER_HOME = '/snap/bin/flutter'
        PATH = "$FLUTTER_HOME/bin:$PATH"
    }

    triggers {
              GenericTrigger(
                      genericVariables: [
                              [key: 'sourceBranch',        value: '$.pull_request.head.ref'],
                              [key: 'destinationBranch',   value: '$.pull_request.base.ref'],
                              [key: 'repoUrl',             value: '$.repository.html_url'],
                              [key: 'targetBranchChanged', value: '$.target_branch.changed']
                      ],
                      causeString              : 'Triggered by Github',
                      regexpFilterText         : ("\$repoUrl"),
                      regexpFilterExpression   : ("https://github.com/DmitrDomrachev/flutter_game"),
                      printContributedVariables: true,
                      printPostContent         : true
              )
    }

  stages {
    stage('Clone GitHub Repository') {
      steps {
        git credentialsId: '58df7ab5-3588-4912-923b-e9dbb1456ef0', url: 'https://github.com/DmitrDomrachev/flutter_game', branch: 'main'
      }
    }

    stage('Flutter Clean') {
      steps {
        sh 'flutter clean'
      }
    }

    stage('Flutter Pub Get') {
      steps {
        sh 'flutter pub get'
      }
    }

    stage('Flutter Test') {
      steps {
        sh 'flutter test'
      }
    }

    stage('Flutter Build Web') {
      steps {
        sh 'flutter build web --base-href /flutter_game/'
      }
    }

  }
}
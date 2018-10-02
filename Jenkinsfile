node {
  def app

  parameters {
  }

  try {
    stage('Clone repository') {
      checkout scm
    }

    stage('Build docker image') {
      // the dockerfile must be the last arg
        withCredentials([[
            $class: 'SSHUserPrivateKeyBinding',
            credentialsId: 'docker_jenkins_key',
            keyFileVariable: 'private_key'
        ]]) {
      // Get the value of the key to pass to docker.
      SSH_VALUE = sh (
        script: '#!/bin/sh -e\n' + 'cat $private_key',
        returnStdout: true
      ).trim()
      app = docker.build("${env.CONTAINER_NAME}:${env.BRANCH}", "--build-arg SSH_KEY_VALUE='${SSH_VALUE}' -f Dockerfile.${env.APPLICATION}.${env.ENV} .")
      }
    }
}

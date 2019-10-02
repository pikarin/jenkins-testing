#!/usr/bin/env groovy

node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Package') {
        sh "echo $JENKINS_HOME"
        sh "echo $WORKSPACE"
        
        sh "rm test-build.tar.gz"
        
        dir("${JENKINS_HOME}/workspace") {
            sh "tar -czvf test-build.tar.gz adit.test --exclude=adit.test/.DS_Store --exclude=adit.test/*.git* --exclude=adit.test/node_modules"
        }
        sh "mv $JENKINS_HOME/workspace/test-build.tar.gz $WORKSPACE/test-build.tar.gz"
        sh "tar -tvf test-build.tar.gz"
    }
    
    stage('Deploy') {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'holahalo-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/holadev/sites/testing
        tar -xzvf test-build.tar.gz

        mv adit.test adit.test$BUILD_ID
        ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'testing', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'test-build.tar.gz')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }
}

#!/usr/bin/env groovy

node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Package') {
        sh "echo $JENKINS_HOME"
        sh "echo $WORKSPACE"
        
        dir("${JENKINS_HOME}/workspace") {
            sh "tar -czvf test-build.tar.gz $WORKSPACE/ --exclude=$WORKSPACE/*.DS_Store --exclude=$WORKSPACE/*.git* --exclude=$WORKSPACE/node_modules"
            sh "mv $JENKINS_HOME/workspace/test-build.tar.gz $WORKSPACE/test-build.tar.gz"
        }
        
        sh "tar -tvf test-build.tar.gz"
    }
    
    stage('Deploy') {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'holahalo-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/holadev/testing

        tar -xzvf test-build.tar.gz

        rm test-build.tar.gz

        cd /home/holadev/testing/adit.test

        cd ..

        mv adit.test adit.test$BUILD_ID
        ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/home/holadev/testing', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'test-build.tar.gz')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }
}

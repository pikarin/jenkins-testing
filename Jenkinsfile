#!/usr/bin/env groovy

node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Package') {
        sh "echo $JENKINS_HOME"
        sh "echo $WORKSPACE"
        sh "which phpcs"
        
        sh "rm test-build.tar.gz --force"
        
        dir("${JENKINS_HOME}/workspace") {
            sh "tar --exclude=.DS_Store --exclude=.git -czvf test-build.tar.gz $JOB_NAME"
        }
        sh "mv $JENKINS_HOME/workspace/test-build.tar.gz $WORKSPACE/test-build.tar.gz"
        sh "tar -tvf test-build.tar.gz"
    }
    
    stage('Deploy') {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'holahalo-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/holadev/sites/testing
        tar -xzvf test-build.tar.gz
        
        rm test-build.tar.gz --force

        mv $JOB_NAME $JOB_NAME$GIT_COMMIT-$BUILD_ID
        ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'testing', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'test-build.tar.gz')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }
}

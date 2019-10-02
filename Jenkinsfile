#!/usr/bin/env groovy

node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Package') {
        sh "echo $JENKINS_HOME"
        sh "echo $WORKSPACE"
        
        dir("${JENKINS_HOME}/workspace") {
            sh "tar -czvf test-build.tar.gz $WORKSPACE/ --exclude=.DS_Store --exclude=.git* --exclude=node_modules"
            sh "mv $JENKINS_HOME/workspace/test-build.tar.gz $WORKSPACE/test-build.tar.gz"
        }
        
        sh "tar -tvf test-build.tar.gz"
    }
    
    stage('Deploy') {
        sshPublisher(publishers: [sshPublisherDesc(configName: 'holahalo-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''tar -xzvf test-build.tar.gz

        mv adit.test adit.test$BUILD_ID
        ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'testing', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'test-build.tar.gz')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    }
}

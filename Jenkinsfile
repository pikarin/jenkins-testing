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
        echo "Done!"
    }
}

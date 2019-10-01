#!/usr/bin/env groovy

node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Package') {
        sh "echo $JENKINS_HOME"
        sh "echo $WORKSPACE"
        sh "cd .."
        sh "tar -czvf test-build.tar.gz $WORKSPACE/ --exclude=$WORKSPACE/*.DS_Store --exclude=$WORKSPACE/*.git* --exclude=$WORKSPACE/node_modules"
        sh "mv ~/workspace/test-build.tar.gz $WORKSPACE/test-build.tar.gz"
        sh "cd $WORKSPACE"
        sh "tar -tvf test-build.tar.gz"
        echo "Done!"
    }
}

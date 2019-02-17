@Library('jenkins-pipeline-utils') _

switch(env.BUILD_JOB_TYPE) {
  case "master": buildMaster(); break;
  case "release":releasePipeline(); break;
  default: buildPullRequest();
}

def buildPullRequest() {
  node('') {
    createStage('Setup')
    stage('Quality Gates') {
      parallel (
        'Unit Test': { createStage('Unit Test') },
        'Lint': { createStage('Lint') }
       )
    }
    createStage('Acceptance Tests')
    /*comment*/
    stage('Test Cleanup') {
      sh 'echo $PATH'
      sh "docker stop java1"
    }
    createStage('SemVer Check')
  }
}

def buildMaster() {
  node('') {
    createStage('Setup')
    stage('Quality Gates') {
      parallel (
        'Unit Test': { createStage('Unit Test') },
        'Lint': { createStage('Lint') }
       )
    }
    createStage('Acceptance Tests')
    createStage('Build Docker Image')
    createStage('Tag Repository')
    createStage('Publish Docker Image')
    createStage('Security Scan')
    createStage('Trigger Release Pipeline')
  }
}

def releasePipeline() {
  node('') {
    createStage('Checkout')
    createStage('Deploy to Dev')
    createStage('Smoke Test')
    createStage('Deploy to Integration')
    createStage('Smoke Test')
    createStage('Acceptance Tests')
    createStage('Deploy to Performance')
    createStage('Smoke Test')
    createStage('Performance Tests')
    createStage('Deploy to Staging')
    createStage('Smoke Test')
    createStage('Deploy Blue/Green Prod')
  }
}

def createStage(name) {
  stage("$name") {
    sh "sleep ${randomSleep()}"
    echo "$name"
  }
}

def randomSleep() {
  (new Random().nextInt().abs() % 10) + 1
}
// Just a comment

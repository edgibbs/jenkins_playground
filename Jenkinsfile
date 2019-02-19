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
    stage('Test Cleanup') {
      ws {
        git branch: 'master', credentialsId: '8da4af92-c9de-409c-856f-ba29ea120d10', url: 'https://github.com/edgibbs/sample-jenkins.git'
        sh 'git status --porcelain --untracked-files=no'
        sh "git config --global user.email fake@fake.net"
        sh "git config --global user.name fake"
        sh "rm test.yaml || true"
        writeYaml file: "test.yaml", data: [ 'key': 'value' ]
        sh "git add test.yaml"
        sh "git commit -am 'Add this'"
        try {
        } catch (Exception e) {
          sh 'git push origin master'
          echo "This blew up ${e}"
        }
      }
      sh 'echo $PATH'
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

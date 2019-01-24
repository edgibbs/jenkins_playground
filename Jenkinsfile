node('') {
  createStage('Setup')
  stage('Quality Gates') {
    parallel (
      unitTest: { createStage('Unit Test') },
      lint: { createStage('Lint') }
    )
  }
  createStage('Acceptance Tests')
  createStage('Build Docker Image')
  createStage('Tag Repository')
  createStage('Publish Docker Image')
  createStage('Trigger Release Pipeline')
}

def createStage(name) {
  stage("$name") {
    echo "$name"
  }
}

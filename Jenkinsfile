node('') {
  createStage('Setup')
  parallel {
    createStage('Unit Test')
    createStage('Lint')
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

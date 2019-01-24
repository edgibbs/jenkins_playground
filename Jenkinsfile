node('') {
  createStage('Setup')
  stage('Quality Gates') {
    parallel (
      'Unit Test': { sh "sleep 3"; createStage('Unit Test') },
      'Lint': { createStage('Lint') }
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

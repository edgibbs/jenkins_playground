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
  createStage('Trigger Release Pipeline')
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

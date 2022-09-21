pipeline {
  agent any
environment {
    SEMGREP_BASELINE_REF = "origin/${env.CHANGE_TARGET}"

    // == Optional settings in the `environment {}` block

    // Instead of `SEMGREP_RULES:`, use rules set in Semgrep App.
    // Get your token from semgrep.dev/manage/settings.
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
      SEMGREP_REPO_URL = env.GIT_URL.replaceFirst(/^(.*).git$/,'$1')
      SEMGREP_BRANCH = "${GIT_BRANCH}"
      SEMGREP_JOB_URL = "${BUILD_URL}"
      SEMGREP_REPO_NAME = env.GIT_URL.replaceFirst(/^https:\/\/github.com\/(.*).git$/, '$1')
      SEMGREP_COMMIT = "${GIT_COMMIT}"
      SEMGREP_PR_ID = "${env.CHANGE_ID}"

    // Change job timeout (default is 1800 seconds; set to 0 to disable)
    //   SEMGREP_TIMEOUT = "300"
  }
  stages {
    stage('Semgrep-Scan') {
        steps {
            sh 'echo $SEMGREP_APP_TOKEN'
            sh 'echo $SEMGREP_REPO_URL'
            sh 'echo $SEMGREP_REPO_NAME'
            sh 'echo $SEMGREP_BRANCH'
            sh 'echo $SEMGREP_COMMIT'
            sh 'ls -la'
            sh 'whoami'
            sh 'newgrp docker'
            sh '''docker pull returntocorp/semgrep && \
    docker run \
    -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
    -e SEMGREP_REPO_URL=$SEMGREP_REPO_URL \
    -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
    -e SEMGREP_REPO_NAME=$SEMGREP_REPO_NAME \
    -e SEMGREP_BRANCH=$SEMGREP_BRANCH \
    -e SEMGREP_COMMIT=$SEMGREP_COMMIT \
    -e SEMGREP_PR_ID=$SEMGREP_PR_ID \
    -v "$(pwd):$(pwd)" --workdir $(pwd) \
    returntocorp/semgrep semgrep ci '''
        }
    }
  }
}

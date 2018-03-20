#!/usr/bin/groovy

pipeline {
  agent {
    label 'Mac'
  }
  options {
    disableConcurrentBuilds()
    timeout(time: 15, unit: 'MINUTES')
  }
  stages {
    stage('Increment Build Number'){
      when {
        allOf {
          // The last commit was not Jenkins...
          expression {
            script {
              sh '''
                set +e
                git log -1 --pretty=%B | grep -q ^Jenkins-Commit; echo $? > status
                set -e
              '''
              def r = readFile('status').trim()
              return r
            }
          }
          //  We are on the dev branch...
          branch 'development'
          // The last build worked...
          expression { return currentBuild.getPreviousBuild().result == 'SUCCESS';}
        }
      }
      steps {
        sh '''
          echo "a loop" >> foo
          git commit foo -m "Jenkins-Commit: set build to $NEW_BUILD"
          git push origin HEAD:$GIT_BRANCH
        '''
      }
    }
  }
}

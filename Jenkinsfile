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
              return=$(git log -1 --pretty=%B | grep -q ^Jenkins-Commit; echo $?)
              set -e
              if [[ $return -eq 1 ]];
              then
              echo 1 > status
              else
              echo 0 > status
              fi
              '''
              def r = readFile('status').trim()
              return r.toString()
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

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
          // Was the last commit Jenkins?
          expression {
            script {
              sh '''
              set +e
              rm -f status
              return=$(git log -1 --pretty=%B | grep -q ^Jenkins-Commit; echo $?)
              set -e
              if [[ $return -eq 1 ]];
              then
              echo "Skip-This-Build" > status
              else
              touch status
              fi
              '''
            }
            def r = readFile('status').trim()
            return r
          }
          //  We are on the dev branch...
        //  branch 'master'
          // The last build worked...
        //  expression { return currentBuild.getPreviousBuild().result == 'SUCCESS';}
        }
      }
      steps {
        sleep 10
        sh '''
          echo "a loop" >> foo
          git commit foo -m "Jenkins-Commit: set build to $NEW_BUILD"
          git push origin HEAD:$GIT_BRANCH
        '''
      }
    }
  }
}

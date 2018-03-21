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
              return=$(git log -1 --pretty=%B | grep -q ^Jenkins-Commit; echo $?)
              set -e
              if [[ $return -eq 0 ]];
              then
              echo True > status
              else
              echo False > status
              fi
              '''
            }
            def r = readFile('status').trim()
            println r
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

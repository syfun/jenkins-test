pipeline {
  agent any
  triggers {
    GenericTrigger(
     genericVariables: [
      [key: 'ref', value: '$.ref'],
      [key: 'repo', value: '$.repository.full_name'],
     ],
     
     causeString: 'Triggered on $ref',
     
     token: env.JOB_NAME,
     
     printContributedVariables: true,
     printPostContent: true,
     
     silentResponse: false,
    
     regexpFilterText: '$ref',
     regexpFilterExpression: '^(refs/heads/(dev|master))$'
    )
  }
  stages {
    stage('Get build parameters') {
      steps {
        echo env.BRANCH_NAME
        sh "printenv"
        // script {
        //     def build = -1
        //     def tag = ''
        //     if ($ref == '/refs/heads/dev') {
        //         tag = 'dev-latest'
        //     } else if ($ref == '/refs/heads/release') {
        //         tag = 'release-latest'
        //     } else if ($ref == '/refs/heads/bugfix') {
        //         tag = 'bugfix-latest'
        //     } else if ($ref == '/refs/heads/hotfix') {
        //         tag = 'hotfix-latest'
        //     } else if ($ref == '/refs/tags/hotfix') {
        //         build = 1
        //     }
        // }
        // echo 'Build dcr.teletraan.io/seely/backend:$tag, and build $build'
      }
    }
  }
}
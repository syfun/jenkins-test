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
        script {
            def build = -1
            def tag = ''
            if ($ref == '/refs/heads/dev') {
                def tag = 'dev-latest'
            } else if ($ref == '/refs/heads/release') {
                def tag = 'release-latest'
            } else if ($ref == '/refs/heads/bugfix') {
                def tag = 'bugfix-latest'
            } else if ($ref == '/refs/heads/hotfix') {
                def tag = 'hotfix-latest'
            }
        }
        echo 'Build dcr.teletraan.io/seely/backend:$tag'
      }
    }
  }
}
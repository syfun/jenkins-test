imageTag = ""

pipeline {
  agent none
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
  environment {
    image = "dcr.teletraan.io/public/jenkins-test"
    tag = ""
  }
  stages {
    stage('Simple build') {
      when {
          anyOf {
              environment name: 'GIT_BRANCH', value: 'origin/dev'
              environment name: 'GIT_BRANCH', value: 'origin/master'
          }
      }
      steps {
        sh "printenv"
        script {
          imageTag = env.GIT_BRANCH.split('/')[1]
          dockerImage = docker.build(registry + ":" + imageTag)
        }
      }
    }
    stage('Deploy image') {
      steps {
        script {
          dockerImage.push()
        }
      }
    }
  }
}
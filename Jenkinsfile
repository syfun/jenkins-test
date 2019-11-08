imageTag = ""

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
  environment {
    image = "dcr.teletraan.io/public/jenkins-test"
    tag = ""
  }
  stages {
    stage('Print env') {
      steps {
        sh 'printenv'
        sh 'echo $ref'
        sh 'echo $repo'
      }
    }
    stage('Simple build') {
      when {
        expression { ref ==~ /refs\/heads\/(dev|hotfix|bugfix)/ }
      }
      steps {
        script {
          dockerImage = docker.build(image + ":" + env.GIT_BRANCH.split('/')[1] + "-latest")
        }
      }
    }
    stage('Compile build') {
      when {
        expression { ref ==~ ref ==~ /refs\/heads\/(release)/ }
      }
      steps {
        script {
          dockerImage = docker.build(image + ":" + env.GIT_BRANCH.split('/')[1] + "-latest")
        }
      }
    }
    stage('Deploy image') {
      when {
        expression { dockerImage }
      }
      steps {
        script {
          dockerImage.push()
        }
      }
    }
  }
}
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
     regexpFilterExpression: '^(refs/heads/(dev|hotfix|bugfix).*|refs/tags/.*)$'
    )
  }
  environment {
    image = 'dcr.teletraan.io/public/jenkins-test'
    tag = ''
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
        expression { ref ==~ /refs\/heads\/(dev|hotfix|bugfix).*/ }
      }
      steps {
        script {
          branch = ref.split('/')[2]
          sh 'git checkout ${branch}'
          dockerImage = docker.build(image + ':' + getImageTag(branch))
        }
      }
    }
    stage('Release build') {
      when {
        expression { ref ==~ /refs\/heads\/release.*/ }
      }
      steps {
        script {
          branch = ref.split('/')[2]
          sh 'git checkout ${branch}'
          dockerImage = docker.build(image + ':' + getImageTag(branch), '--build-arg compile=1')
        }
      }
    }
    stage('Tag build') {
      when {
        expression { ref ==~ /refs\/tags\/.*/ }
      }
      steps {
        script {
          tag = ref.split('/')[2]
          sh 'git checkout -b ${tag} ${tag}'
          dockerImage = docker.build(image + ':' + tag, '--build-arg compile=1')
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

def getImageTag(branch) {
  if (branch.startsWith('dev')) {
    return 'dev-latest'
  } else if (branch.startsWith('release')) {
    return 'release-latest'
  } else if (branch.startsWith('bugfix')) {
    return 'release-latest'
  } else if (branch.startsWith('hotfix')) {
    return 'release-latest'
  }
}
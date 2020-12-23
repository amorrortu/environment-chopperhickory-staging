pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "jx-staging"
  }
  stages {
    stage('Validate Environment') {
      steps {
        container('maven') {
          dir('env') {
            sh "helm init --client-only --stable-repo-url https://charts.helm.sh/stable"
            sh 'jx step helm build'
          }
        }
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh "helm init --client-only --stable-repo-url https://charts.helm.sh/stable"
            sh 'jx step helm apply'
          }
        }
      }
    }
  }
}

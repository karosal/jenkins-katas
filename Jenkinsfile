pipeline {
  agent any
  options {
    skipDefaultCheckout()
  }
  stages {
    stage('Clone down') {
      agent { label 'swarm' }
      steps {
        stash excludes: '.git', name: 'code'
      }
    }
    stage('Hello world') {
      parallel {
        stage('Parallel execution') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          steps {
            unstash 'code'
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
          }
        }

      }
    }

  }
}
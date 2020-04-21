pipeline {
  agent {
    docker { image 'maven:3-alpine' }
  }
  stages {
    stage('default') {
      parallel {
        stage('build'){
          steps {
            script {
              try {
                sh '''
                  echo "build (failing)!"
                  touch ./TEST-io.helidon.build.publisher.model.PipelineRunTest.xml
                  sleep 30
                '''
              } finally {
                archiveArtifacts artifacts: "**/*.xml"
                junit testResults: 'TEST-io.helidon.build.publisher.model.PipelineRunTest.xml'
              }
            }
          }
        }
        stage('copyright'){
          steps {
            sh 'echo "copyright!" ; exit 1'
          }
        }
        stage('checkstyle'){
          steps {
            sh 'echo "checkstyle!"'
          }
        }
      }
    }
    stage('release') {
      when {
        branch '**/release-*'
      }
      steps {
        sh '''
            echo "release!"
        '''
      }
    }
  }
}
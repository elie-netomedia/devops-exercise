pipeline {
  agent { docker { image 'python:3.8' } }
  stages {
    stage('build') {
      steps {
        sh 'pip install -r requirements.txt --user'
      }
    }
    stage('test') {
      steps {
        sh 'python unittest.py'
      }
      post {
        always {
          junit 'results.xml'
        }
      }    
    }
  }
}